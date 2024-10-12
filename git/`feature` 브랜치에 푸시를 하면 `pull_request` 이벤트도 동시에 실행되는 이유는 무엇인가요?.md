### **질문**: `feature` 브랜치에 푸시를 하면 `pull_request` 이벤트도 동시에 실행되는 이유는 무엇인가요?

```
on:
  push:
    branches:
      - feature  # feature 브랜치에 푸시될 때 실행
  pull_request:
    branches:
      - develop  # develop 브랜치로의 풀 리퀘스트 시 실행
```



## GitHub Actions 이벤트 트리거 동작 방식

GitHub Actions에서 워크플로우는 지정된 이벤트(`push`, `pull_request` 등)에 반응하여 실행됩니다. 여기서 중요한 점은 특정 브랜치에 대한 푸시가 다른 이벤트를 어떻게 트리거할 수 있는지입니다.

### 1. **`push` 이벤트**

- **트리거 조건**: `feature` 브랜치에 새로운 커밋이 푸시(push)될 때.
- **동작**: 푸시가 완료된 후, `push` 이벤트에 정의된 워크플로우가 실행됩니다.

### 2. **`pull_request` 이벤트**

- 트리거 조건

  : 

  ```
  develop
  ```

   브랜치로의 풀 리퀘스트(PR)가 생성되거나 업데이트될 때.

  - PR의 **대상 브랜치(target branch)**가 `develop`인 경우.

- **동작**: PR이 생성되거나, PR의 소스 브랜치(`feature`)에 새로운 커밋이 추가될 때 워크플로우가 실행됩니다.

### 3. **두 이벤트의 상호작용**

#### **시나리오 1: PR이 없는 경우**

1. 푸시(Push)

   :

   - `feature` 브랜치에 커밋을 푸시하면 **`push` 이벤트**만 트리거됩니다.
   - **`pull_request` 이벤트**는 트리거되지 않습니다.

#### **시나리오 2: PR이 이미 열려 있는 경우**

1. PR 열기

   :

   - `feature` 브랜치에서 `develop` 브랜치로 PR을 생성합니다.

2. 푸시(Push)

   :

   - ```
     feature
     ```

      브랜치에 새로운 커밋을 푸시하면:

     - **`push` 이벤트**가 트리거됩니다.
     - 동시에, PR이 업데이트되므로 **`pull_request` 이벤트**도 트리거됩니다.

이렇게 하면 동일한 푸시가 두 가지 이벤트를 모두 트리거하게 됩니다.

## 왜 두 이벤트가 동시에 트리거될까?

### **PR 업데이트의 특성**

- PR은 소스 브랜치(`feature`)와 대상 브랜치(`develop`) 간의 통합 요청입니다.
- 소스 브랜치에 새로운 커밋이 추가되면, PR은 해당 변경 사항을 반영하도록 업데이트됩니다.
- 이 업데이트는 PR 자체의 변경으로 간주되며, GitHub은 이를 **`pull_request` 이벤트**로 인식합니다.

### **워크플로우 트리거 설정**

- `push` 이벤트

  :

  - 특정 브랜치(`feature`)에 푸시가 발생하면 워크플로우가 실행됩니다.

- `pull_request` 이벤트

  :

  - 대상 브랜치(`develop`)로의 PR이 생성되거나 업데이트될 때 워크플로우가 실행됩니다.

따라서, `feature` 브랜치에 푸시하면:

1. **`push` 이벤트**가 트리거되어 워크플로우가 실행됩니다.
2. 동시에, PR이 업데이트되므로 **`pull_request` 이벤트**도 트리거되어 워크플로우가 실행됩니다.

이는 GitHub Actions의 기본 동작 방식이며, PR과 관련된 워크플로우가 푸시와 PR 업데이트 모두에 반응하도록 설계된 것입니다.

## 예시 상황 설명

### **상황 1: PR이 없는 상태에서 `feature` 브랜치에 푸시**

1. 작업

   :

   - `feature` 브랜치에 새로운 커밋을 푸시합니다.

2. 트리거된 이벤트

   :

   - **`push` 이벤트**만 트리거됩니다.

3. 실행되는 워크플로우

   :

   - `push`에 정의된 워크플로우가 실행됩니다.

### **상황 2: PR이 열려 있는 상태에서 `feature` 브랜치에 푸시**

1. 작업

   :

   - `feature` 브랜치에 새로운 커밋을 푸시합니다.

2. 트리거된 이벤트

   :

   - **`push` 이벤트**가 트리거됩니다.
   - **`pull_request` 이벤트**도 트리거됩니다.

3. 실행되는 워크플로우

   :

   - `push`에 정의된 워크플로우가 실행됩니다.
   - `pull_request`에 정의된 워크플로우가 실행됩니다.

이로 인해 동일한 푸시 작업이 두 번의 워크플로우 실행을 초래할 수 있습니다.

## 워크플로우 실행을 제어하는 방법

만약 **`push` 이벤트와 `pull_request` 이벤트가 동일한 워크플로우 파일에서 중복 실행되는 것을 방지**하고 싶다면, 몇 가지 방법을 사용할 수 있습니다.

### 1. **워크플로우 파일 분리**

`push` 이벤트와 `pull_request` 이벤트를 **별도의 워크플로우 파일**로 분리하여 관리할 수 있습니다. 이를 통해 각 이벤트에 맞는 워크플로우를 독립적으로 실행할 수 있습니다.

#### **예시: `push.yml`과 `pull_request.yml`로 분리**

**`push.yml`**

```
yaml코드 복사name: Push Build

on:
  push:
    branches:
      - feature

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      - name: Set up JDK 21
        uses: actions/setup-java@v3
        with:
          distribution: 'temurin'
          java-version: '21'
      - name: Cache Gradle dependencies
        uses: actions/cache@v3
        with:
          path: |
            ~/.gradle/caches
            ~/.gradle/wrapper/
          key: gradle-cache-${{ runner.os }}-${{ hashFiles('**/*.gradle*', '**/gradle-wrapper.properties') }}
          restore-keys: |
            gradle-cache-${{ runner.os }}-
      - name: Make gradlew executable
        run: chmod +x ./gradlew
      - name: Build and Test with Gradle
        run: ./gradlew build
```

**`pull_request.yml`**

```
yaml코드 복사name: PR Build and Deploy

on:
  pull_request:
    branches:
      - develop

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      - name: Set up JDK 21
        uses: actions/setup-java@v3
        with:
          distribution: 'temurin'
          java-version: '21'
      - name: Cache Gradle dependencies
        uses: actions/cache@v3
        with:
          path: |
            ~/.gradle/caches
            ~/.gradle/wrapper/
          key: gradle-cache-${{ runner.os }}-${{ hashFiles('**/*.gradle*', '**/gradle-wrapper.properties') }}
          restore-keys: |
            gradle-cache-${{ runner.os }}-
      - name: Make gradlew executable
        run: chmod +x ./gradlew
      - name: Build and Test with Gradle
        run: ./gradlew build
      - name: Deploy to Server
        run: ./deploy.sh
        env:
          API_KEY: ${{ secrets.API_KEY }}
```

이렇게 분리하면, `push` 이벤트와 `pull_request` 이벤트가 서로 독립적으로 워크플로우를 실행하게 됩니다.

### 2. **조건부 실행(`if` 구문) 활용**

단일 워크플로우 파일에서 조건부 실행을 통해 특정 이벤트에만 일부 단계를 실행하도록 설정할 수 있습니다. 이를 통해 중복 실행을 방지할 수 있습니다.

#### **예제: PR 이벤트일 때만 특정 단계를 실행**

```
yaml코드 복사name: Java CI with Gradle

on:
  push:
    branches:
      - feature
  pull_request:
    branches:
      - develop

jobs:
  build:
    runs-on: ubuntu-latest
    env:
      GRADLE_OPTS: "-Dorg.gradle.daemon=false"

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Set up JDK 21
        uses: actions/setup-java@v3
        with:
          distribution: 'temurin'
          java-version: '21'

      - name: Cache Gradle dependencies
        uses: actions/cache@v3
        with:
          path: |
            ~/.gradle/caches
            ~/.gradle/wrapper/
          key: gradle-cache-${{ runner.os }}-${{ hashFiles('**/*.gradle*', '**/gradle-wrapper.properties') }}
          restore-keys: |
            gradle-cache-${{ runner.os }}-

      - name: Make gradlew executable
        run: chmod +x ./gradlew

      - name: Build and Test with Gradle
        run: ./gradlew build

      # 조건부 실행: PR 이벤트인 경우에만 배포 단계 실행
      - name: Deploy (PR Only)
        if: github.event_name == 'pull_request'
        run: ./deploy.sh
        env:
          API_KEY: ${{ secrets.API_KEY }}
```

이 예제에서는 `Deploy` 단계가 **`pull_request` 이벤트**일 때만 실행되도록 조건을 설정하였습니다. 따라서, `push` 이벤트일 때는 `Deploy` 단계가 실행되지 않아 중복 실행을 방지할 수 있습니다.

### 3. **워크플로우 중복 실행 방지**

워크플로우가 동시에 여러 번 실행되는 것을 방지하기 위해, 이전 워크플로우가 완료될 때까지 대기하거나 취소하는 전략을 사용할 수 있습니다. 예를 들어, `workflow_run` 트리거를 사용하여 특정 워크플로우가 완료된 후에만 다른 워크플로우를 실행하도록 설정할 수 있습니다.

#### **예제: 이전 워크플로우 완료 후 실행**

```
yaml코드 복사name: PR Deploy

on:
  workflow_run:
    workflows: ["Push Build"]
    types:
      - completed

jobs:
  deploy:
    if: ${{ github.event.workflow_run.conclusion == 'success' }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      - name: Deploy to Server
        run: ./deploy.sh
        env:
          API_KEY: ${{ secrets.API_KEY }}
```

이렇게 하면, `Push Build` 워크플로우가 성공적으로 완료된 후에만 `PR Deploy` 워크플로우가 실행됩니다.

## 추가 고려사항

### 1. **브랜치 보호 규칙 설정**

브랜치 보호 규칙을 설정하여 특정 조건(예: 워크플로우 성공)을 충족해야만 브랜치로 병합할 수 있도록 제한할 수 있습니다. 이를 통해 빌드 실패 시 PR 병합을 자동으로 차단할 수 있습니다.

#### **설정 방법**

1. **리포지토리 설정으로 이동**:
   - GitHub에서 해당 리포지토리로 이동한 후, 우측 상단의 `Settings` 탭을 클릭합니다.
2. **Branches 섹션으로 이동**:
   - 왼쪽 사이드바에서 `Branches`를 클릭합니다.
3. **브랜치 보호 규칙 추가**:
   - `Add rule` 버튼을 클릭합니다.
   - `Branch name pattern`에 `develop`을 입력합니다.
   - `Require status checks to pass before merging`을 활성화하고, 해당 워크플로우의 빌드 체크를 선택합니다.
   - 추가적인 보호 옵션을 설정한 후 `Create` 버튼을 클릭합니다.

이렇게 설정하면, `develop` 브랜치로의 PR이 성공적인 빌드 상태를 충족하지 않으면 병합할 수 없습니다.

### 2. **워크플로우 파일 분리의 장단점**

#### **장점**

- **명확성**: 각 이벤트에 맞는 워크플로우가 별도로 관리되어, 각 워크플로우의 목적과 동작이 명확해집니다.
- **유지보수 용이성**: 특정 이벤트에 대한 변경 사항을 독립적으로 관리할 수 있어, 유지보수가 용이합니다.
- **중복 방지**: 동일한 워크플로우 파일에서 두 이벤트가 동시에 실행되어 발생할 수 있는 중복 실행을 방지할 수 있습니다.

#### **단점**

- **파일 관리 증가**: 이벤트마다 별도의 워크플로우 파일을 관리해야 하므로, 파일 수가 늘어날 수 있습니다.
- **공통 단계 중복**: 여러 워크플로우 파일에서 공통으로 사용하는 단계가 있을 경우, 이를 중복으로 작성해야 할 수 있습니다.

### 3. **공통 단계의 재사용**

공통으로 사용하는 단계를 재사용 가능한 액션(Action)이나 템플릿으로 만들어 여러 워크플로우에서 참조할 수 있습니다. 이를 통해 코드 중복을 줄이고, 유지보수를 쉽게 할 수 있습니다.

#### **예제: 공통 단계 템플릿 사용**

**공통 단계 템플릿 (`.github/workflows/.common.yml`)**

```
yaml코드 복사# .github/workflows/.common.yml
name: Common Steps

on: workflow_call

jobs:
  setup:
    runs-on: ubuntu-latest
    outputs:
      java-version: ${{ steps.setup-java.outputs.java-version }}
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Set up JDK 21
        id: setup-java
        uses: actions/setup-java@v3
        with:
          distribution: 'temurin'
          java-version: '21'

      - name: Cache Gradle dependencies
        uses: actions/cache@v3
        with:
          path: |
            ~/.gradle/caches
            ~/.gradle/wrapper/
          key: gradle-cache-${{ runner.os }}-${{ hashFiles('**/*.gradle*', '**/gradle-wrapper.properties') }}
          restore-keys: |
            gradle-cache-${{ runner.os }}-

      - name: Make gradlew executable
        run: chmod +x ./gradlew
```

**워크플로우 파일에서 템플릿 호출**

**`push.yml`**

```
yaml코드 복사name: Push Build

on:
  push:
    branches:
      - feature

jobs:
  build:
    uses: ./.github/workflows/.common.yml

  build-and-test:
    needs: setup
    runs-on: ubuntu-latest
    steps:
      - name: Build and Test with Gradle
        run: ./gradlew build
```

**`pull_request.yml`**

```
yaml코드 복사name: PR Build and Deploy

on:
  pull_request:
    branches:
      - develop

jobs:
  build:
    uses: ./.github/workflows/.common.yml

  build-and-test:
    needs: setup
    runs-on: ubuntu-latest
    steps:
      - name: Build and Test with Gradle
        run: ./gradlew build

  deploy:
    needs: build-and-test
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to Server
        run: ./deploy.sh
        env:
          API_KEY: ${{ secrets.API_KEY }}
```

이렇게 하면, 공통 단계를 별도의 템플릿으로 관리하여 각 워크플로우에서 재사용할 수 있습니다.

## 결론

### 요약

- **PR이 없는 경우**: `feature` 브랜치에 푸시하면 **`push` 이벤트**만 트리거됩니다.
- **PR이 이미 열려 있는 경우**: `feature` 브랜치에 푸시하면 **`push` 이벤트**와 **`pull_request` 이벤트**가 모두 트리거됩니다.
- **이유**: `feature` 브랜치에 푸시하면, 해당 브랜치가 열려 있는 PR의 소스 브랜치로 업데이트되므로, PR이 업데이트된 것으로 간주되어 `pull_request` 이벤트도 트리거됩니다.
- **워크플로우 관리**: 이벤트별로 워크플로우 파일을 분리하거나, 조건부 실행을 활용하여 중복 실행을 방지할 수 있습니다.
- **브랜치 보호 규칙**: 빌드 성공 여부에 따라 PR 병합을 제어하여 코드의 안정성을 유지할 수 있습니다.

### 추가 팁

- **로컬에서 사전 검증**: 푸시 전에 로컬에서 빌드와 테스트를 실행하여 가능한 빌드 실패를 미리 방지합니다.
- **자동화된 알림**: 빌드 실패 시 Slack, 이메일 등으로 알림을 설정하여 신속하게 대응할 수 있도록 합니다.
- **워크플로우 최적화**: 필요한 단계만 실행되도록 워크플로우를 최적화하여 빌드 시간을 단축합니다.
