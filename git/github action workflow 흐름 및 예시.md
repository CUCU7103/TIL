# github action workflow 흐름 및 예시



Github Actions 워크플로우 흐름 

> [!IMPORTANT]
>
> 특정 이벤트에 반응하여 자동으로 실행되어지는 job과  step으로 구성되어집니다.



#### 1. Event 트리거

- 워크플로우는 특정 이벤트가 발생할 때 시작되어집니다.
  - 예를들자면 아래와 같은 경우들이 있습니다.
    - 코드가 푸쉬되어졌을때 (push)
    - 풀 리퀘스트가 생성됬을때 (pull_request)
    - 일정한 시간마다(cron)
    - 수동으로 워크플로우를 실행할때 

- 이벤트는  `.github/workflows/` 디렉토리에 위치한 YAML 파일로 정의됩니다.

- 로컬에서의 디렉터리 지정

​	![image-20241012144503923](C:\Users\syste\AppData\Roaming\Typora\typora-user-images\image-20241012144503923.png)

- github 에서의 디렉터리

  ![image-20241012144721935](C:\Users\syste\AppData\Roaming\Typora\typora-user-images\image-20241012144721935.png)![image-20241012144740043](C:\Users\syste\AppData\Roaming\Typora\typora-user-images\image-20241012144740043.png)



#### 2. **워크플로우 정의(Workflow Definition)**:

- 워크플로우는 YAML 형식으로 작성되며, 워크플로우의 이름, 트리거 이벤트, 그리고 실행될 Job과 Step을 정의합니다.
- 워크플로우는 하나 이상의 작업으로 구성된 구성 가능한 자동화된 프로세스입니다. 워크플로우 구성을 정의하려면 YAML 파일을 만들어야 합니다.

- 1) **Workflow**

  - 여러 Job으로 구성되고, Event에 의해 트리거될 수 있는 자동화된 프로세스
  - 최상위 개념
  - Workflow 파일은 YAML으로 작성되고, Github Repository의 `.github/workflows` 폴더 아래에 저장됨

  

- 2) **Event**

  - Workflow를 Trigger(실행)하는 특정 활동이나 규칙
  - 예를 들어 다음과 같은 상황을 사용할 수 있음
    - 특정 브랜치로 Push하거나
    - 특정 브랜치로 Pull Request하거나
    - 특정 시간대에 반복(Cron)
    - Webhook을 사용해 외부 이벤트를 통해 실행
  - 자세한 내용은 [Events that trigger workflows](https://help.github.com/en/actions/reference/events-that-trigger-workflows#external-events-repository_dispatch) 참고

  

- 3) **Job**

  - Job은 여러 Step으로 구성되고, 가상 환경의 인스턴스에서 실행됨
  - 다른 Job에 의존 관계를 가질 수 있고, 독립적으로 병렬 실행도 가능함

  

- 4) **Step**

  - Task들의 집합으로, 커맨드를 날리거나 action을 실행할 수 있음

  

- 5) **Action**

  - Workflow의 가장 작은 블럭(smallest portable building block)

  - Job을 만들기 위해 Step들을 연결할 수 있음

  - 재사용이 가능한 컴포넌트

  - 개인적으로 만든 Action을 사용할 수도 있고, Marketplace에 있는 공용 Action을 사용할 수도 있음

    - [Github Marketplace](https://github.com/marketplace?type=actions)와 [Github Actions Repository](https://github.com/actions/)에서 확인 가능

    

- 6) **Runner**

  - Gitbub Action Runner 어플리케이션이 설치된 머신으로, Workflow가 실행될 인스턴스
  - Github에서 호스팅해주는 Github-hosted runner와 직접 호스팅하는 Self-hosted runner로 나뉨
  - Github-hosted runner는 Azure의 Standard_DS2_v2로 vCPU 2, 메모리 7GB, 임시 스토리지 14GB
    - [Self-hosted runners](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/adding-self-hosted-runners) 참고



기본적인 워크플로우 구조

```yaml
name: 워크플로우 이름

on: 이벤트

jobs:
  job_id:
    runs-on: 실행 환경
    steps:
      - name: 단계 이름
        uses: 액션 또는 액션 참조
        with:
          key: value
      - name: 다른 단계
        run: 실행할 명령어

```



### 1. `name`: 워크플로우 이름

```
yaml


코드 복사
name: Build and Test
```

- **설명**: 워크플로우의 이름을 정의합니다. GitHub Actions 인터페이스에서 이 이름으로 워크플로우를 식별할 수 있습니다.

- 예시

  ```
  yaml
  
  
  코드 복사
  name: CI Pipeline
  ```



### 2. `on`: 이벤트 트리거

```
yaml코드 복사on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main
```

- **설명**: 워크플로우가 언제 실행될지를 정의합니다. 다양한 이벤트와 조건을 설정할 수 있습니다.

- **주요 이벤트**:

  - `push`: 특정 브랜치에 푸시될 때
  - `pull_request`: 풀 리퀘스트가 생성, 업데이트될 때
  - `schedule`: 일정에 따라 (cron 표현식 사용)
  - `workflow_dispatch`: 수동으로 워크플로우를 실행할 때
  - 기타 GitHub에서 지원하는 이벤트들

- **예시**:

  ```
  yaml코드 복사on:
    push:
      branches:
        - develop
    pull_request:
      branches:
        - develop
    schedule:
      - cron: '0 0 * * *' # 매일 자정에 실행
  ```



### 3. `jobs`: 작업 정의

```
yaml코드 복사jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      - name: Run build
        run: ./build.sh
```

- **설명**: 워크플로우 내에서 실행될 작업(Job)을 정의합니다. 각 Job은 독립된 실행 환경에서 동작하며, 병렬 또는 순차적으로 실행될 수 있습니다.

- **주요 속성**:

  - `runs-on`: Job이 실행될 가상 환경 (예: `ubuntu-latest`, `windows-latest`, `macos-latest`)
  - `needs`: 다른 Job에 대한 의존성을 설정하여 순차 실행을 제어
  - `strategy`: 매트릭스 빌드를 설정하여 여러 환경에서 병렬 실행

- **예시**:

  ```
  yaml코드 복사jobs:
    test:
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v3
        - name: Set up Node.js
          uses: actions/setup-node@v3
          with:
            node-version: '14'
        - run: npm install
        - run: npm test
    deploy:
      needs: test
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v3
        - name: Deploy
          run: ./deploy.sh
  ```



물론입니다! GitHub Actions 워크플로우(YAML 파일)의 문법에 대해 자세히 설명드리겠습니다. GitHub Actions 워크플로우는 YAML(YAML Ain't Markup Language) 형식으로 작성되며, 특정 이벤트에 반응하여 일련의 작업(Job)을 자동으로 실행합니다. 아래에서는 GitHub 워크플로우의 기본 구조와 주요 문법 요소에 대해 설명하겠습니다.

## GitHub Actions 워크플로우 기본 구조

GitHub 워크플로우 파일은 일반적으로 `.github/workflows/` 디렉토리에 위치하며, `.yml` 또는 `.yaml` 확장자를 가집니다. 기본적인 워크플로우 파일의 구조는 다음과 같습니다:

```
name: 워크플로우 이름

on: 이벤트

jobs:
  job_id:
    runs-on: 실행 환경
    steps:
      - name: 단계 이름
        uses: 액션 또는 액션 참조
        with:
          key: value
      - name: 다른 단계
        run: 실행할 명령어
```

각 섹션에 대해 자세히 살펴보겠습니다.

------

### 1. `name`: 워크플로우 이름

```
yaml


코드 복사
name: Build and Test
```

- **설명**: 워크플로우의 이름을 정의합니다. GitHub Actions 인터페이스에서 이 이름으로 워크플로우를 식별할 수 있습니다.

- 예시

  :

  ```
  yaml
  
  
  코드 복사
  name: CI Pipeline
  ```

------

### 2. `on`: 이벤트 트리거

```
on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main
```

- **설명**: 워크플로우가 언제 실행될지를 정의합니다. 다양한 이벤트와 조건을 설정할 수 있습니다.

- **주요 이벤트**:

  - `push`: 특정 브랜치에 푸시될 때
  - `pull_request`: 풀 리퀘스트가 생성, 업데이트될 때
  - `schedule`: 일정에 따라 (cron 표현식 사용)
  - `workflow_dispatch`: 수동으로 워크플로우를 실행할 때
  - 기타 GitHub에서 지원하는 이벤트들

- **예시**:

  ```
  on:
    push:
      branches:
        - develop
    pull_request:
      branches:
        - develop
    schedule:
      - cron: '0 0 * * *' # 매일 자정에 실행
  ```

------

### 3. `jobs`: 작업 정의

```
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      - name: Run build
        run: ./build.sh
```

- **설명**: 워크플로우 내에서 실행될 작업(Job)을 정의합니다. 각 Job은 독립된 실행 환경에서 동작하며, 병렬 또는 순차적으로 실행될 수 있습니다.

- **주요 속성**:

  - `runs-on`: Job이 실행될 가상 환경 (예: `ubuntu-latest`, `windows-latest`, `macos-latest`)
  - `needs`: 다른 Job에 대한 의존성을 설정하여 순차 실행을 제어
  - `strategy`: 매트릭스 빌드를 설정하여 여러 환경에서 병렬 실행

- **예시**:

  ```
  jobs:
    test:
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v3
        - name: Set up Node.js
          uses: actions/setup-node@v3
          with:
            node-version: '14'
        - run: npm install
        - run: npm test
    deploy:
      needs: test
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v3
        - name: Deploy
          run: ./deploy.sh
  ```

------



### 4. `steps`: 단계 정의

각 Job 내에서 실행될 단계(Step)를 정의합니다. Step은 순차적으로 실행되며, 각각의 Step은 명령어를 실행하거나 액션을 호출할 수 있습니다.

```
steps:
  - name: 단계 이름
    uses: 액션 참조 또는 `actions/checkout@v3`
    with:
      key: value
  - name: 다른 단계
    run: 실행할 명령어
```

- **주요 속성**:

  - `name`: 단계의 이름 (선택적)
  - `uses`: GitHub 액션을 사용하여 단계 실행
  - `run`: 직접 명령어를 실행
  - `with`: 액션에 전달할 입력 매개변수
  - `env`: 단계에서 사용할 환경 변수 설정
  - `shell`: 사용할 쉘 지정 (기본값은 `bash`)

- **예시**:

  ```
  steps:
    - name: Checkout repository
      uses: actions/checkout@v3
  
    - name: Set up Python
      uses: actions/setup-python@v4
      with:
        python-version: '3.8'
  
    - name: Install dependencies
      run: pip install -r requirements.txt
  
    - name: Run tests
      run: pytest
  ```

---



### 5. `uses`와 `run`: 액션과 명령어 실행

- **`uses`**: 재사용 가능한 GitHub 액션을 호출합니다. 주로 `actions/checkout@v3`처럼 액션 레포지토리와 버전을 지정합니다.

  ```
  - name: Checkout code
    uses: actions/checkout@v3
  ```

- **`run`**: 직접 명령어를 실행합니다. 쉘 명령어를 사용하여 필요한 작업을 수행할 수 있습니다.

  ```
  - name: Install dependencies
    run: npm install
  ```

------



### 6. `with`: 액션에 입력 값 전달

`uses`로 호출한 액션에 추가적인 설정이나 입력 값을 전달할 때 사용합니다.

```
- name: Set up JDK
  uses: actions/setup-java@v3
  with:
    distribution: 'temurin'
    java-version: '17'
```

- **설명**: `setup-java` 액션에 `distribution`과 `java-version`을 전달하여 JDK 환경을 설정합니다.

------



### 7. `env`: 환경 변수 설정

워크플로우, Job, Step 수준에서 환경 변수를 설정할 수 있습니다.

```
env:
  NODE_ENV: production
```

- **설명**: 모든 Job과 Step에서 사용할 수 있는 환경 변수를 정의합니다.

- **예시**:

  ```
  jobs:
    build:
      runs-on: ubuntu-latest
      env:
        NODE_ENV: production
      steps:
        - name: Checkout code
          uses: actions/checkout@v3
        - name: Install dependencies
          run: npm install
        - name: Build
          run: npm run build
  ```

------



### 8. `secrets`: 시크릿 관리

GitHub Secrets를 사용하여 민감한 정보를 안전하게 관리할 수 있습니다. 시크릿은 워크플로우 내에서 환경 변수로 사용할 수 있습니다.

```
- name: Deploy
  run: ./deploy.sh
  env:
    API_KEY: ${{ secrets.API_KEY }}
```

- **설명**: `secrets.API_KEY`에 저장된 민감한 정보를 `API_KEY` 환경 변수로 사용합니다.

------



### 9. `strategy`: 매트릭스 빌드 설정

여러 환경에서 병렬로 Job을 실행할 때 사용합니다. 주로 다양한 OS나 언어 버전에서 테스트할 때 유용합니다.

```
jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [12, 14, 16]
    steps:
      - uses: actions/checkout@v3
      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.node-version }}
      - run: npm install
      - run: npm test
```

- **설명**: `node-version`의 각 값(12, 14, 16)에 대해 별도의 Job이 병렬로 실행됩니다.

------



### 10. `needs`: Job 간의 의존성 설정

한 Job이 다른 Job의 완료를 기다려야 할 때 사용합니다.

```
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - run: make build

  test:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - run: make test
```

- **설명**: `test` Job은 `build` Job이 완료된 후에 실행됩니다.

------



### 11. `if`: 조건부 실행

특정 조건이 만족될 때만 Job이나 Step을 실행하도록 설정할 수 있습니다.

```
jobs:
  deploy:
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - run: ./deploy.sh
```

- **설명**: 현재 브랜치가 `main`일 때만 `deploy` Job이 실행됩니다.

------



### 12. `outputs`와 `needs`: Job 간 데이터 전달

한 Job의 결과를 다른 Job에서 사용할 수 있습니다.

```
jobs:
  build:
    runs-on: ubuntu-latest
    outputs:
      artifact_path: ${{ steps.build.outputs.artifact_path }}
    steps:
      - uses: actions/checkout@v3
      - name: Build
        id: build
        run: echo "::set-output name=artifact_path::build/output/path"

  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Deploy
        run: ./deploy.sh ${{ needs.build.outputs.artifact_path }}
```

- **설명**: `build` Job에서 생성된 `artifact_path`를 `deploy` Job에서 사용합니다.



## 전체 예제 워크플로우 (Java & Gradle 기준)

```
# 워크플로우의 이름을 정의합니다.
name: Java CI with Gradle

# 워크플로우가 언제 실행될지를 정의합니다.
on:
  push:
    branches:
      - feature  # feature 브랜치에 푸시될 때 실행
  pull_request:
    branches:
      - develop  # develop 브랜치로의 풀 리퀘스트 시 실행

# 워크플로우 내에서 실행될 작업들을 정의합니다.
jobs:
  build:

    # 작업이 실행될 운영 체제를 정의합니다.
    runs-on: ubuntu-latest

    # Job 내에서 사용할 환경 변수를 정의합니다.
    env:
      GRADLE_OPTS: "-Dorg.gradle.daemon=false"

    # 작업의 단계들을 정의합니다.
    steps:
      # 1. 코드 체크아웃
      - name: Checkout code
        uses: actions/checkout@v3

      # 2. JDK 설정
      - name: Set up JDK 21
        uses: actions/setup-java@v3
        with:
          distribution: 'temurin'  # Temurin 배포판 사용
          java-version: '21'        # JDK 버전 21

      # 3. Gradle 캐싱 설정
      - name: Cache Gradle dependencies
        uses: actions/cache@v3
        with:
          path: |
            ~/.gradle/caches
            ~/.gradle/wrapper/
          key: gradle-cache-${{ runner.os }}-${{ hashFiles('**/*.gradle*', '**/gradle-wrapper.properties') }}
          restore-keys: |
            gradle-cache-${{ runner.os }}-

      # 4. Gradle 빌드 파일 실행 권한 부여
      - name: Make gradlew executable
        run: chmod +x ./gradlew

      # 5. Gradle 빌드 및 테스트 실행
      - name: Build and Test with Gradle
        run: ./gradlew build
```



## 워크플로우 구성 요소별 설명

### 1. 워크플로우 이름 (`name`)

```
name: Java CI with Gradle
```

- **설명**: 이 워크플로우의 이름을 **Java CI with Gradle**로 정의합니다. GitHub Actions 인터페이스에서 이 이름으로 워크플로우를 식별할 수 있습니다.

### 2. 이벤트 트리거 (`on`)

```
on:
  push:
    branches:
      - feature
  pull_request:
    branches:
      - develop
```

- **설명**: 워크플로우가 언제 실행될지를 정의합니다.
  - push 이벤트:
    - `branches: - feature`: **feature** 브랜치에 코드가 푸시(push)될 때 워크플로우가 실행됩니다.
  - pull_request 이벤트:
    - `branches: - develop`: **develop** 브랜치로의 풀 리퀘스트(PR)가 생성되거나 업데이트될 때 워크플로우가 실행됩니다.
- **요약**:
  - **feature 브랜치**에 코드가 푸시될 때
  - **develop 브랜치**로 풀 리퀘스트가 생성되거나 업데이트될 때 워크플로우가 실행됩니다.

### 3. 작업(Job) 정의 (`jobs`)

```
jobs:
  build:
    runs-on: ubuntu-latest
    env:
      GRADLE_OPTS: "-Dorg.gradle.daemon=false"
    steps:
      # 단계들...
```

- **jobs**: 워크플로우 내에서 실행될 작업들의 집합입니다.
- **build**: 이 워크플로우에서 정의된 단일 작업(Job)입니다.
- **runs-on: ubuntu-latest**: 이 작업은 최신 버전의 **Ubuntu** 가상 환경에서 실행됩니다. GitHub는 여러 운영 체제를 지원하며, `ubuntu-latest`는 최신 안정화된 Ubuntu 버전을 사용합니다.
- **env**: Job 전체에서 사용할 환경 변수를 정의합니다.
  - `GRADLE_OPTS: "-Dorg.gradle.daemon=false"`: Gradle 데몬을 비활성화하여 빌드 일관성을 높입니다.

### 4. 단계(Steps) 정의 (`steps`)

작업 내에서 순차적으로 실행될 단계들을 정의합니다.

#### 4.1. 코드 체크아웃 (`Checkout code`)

```
- name: Checkout code
  uses: actions/checkout@v3
```

- **설명**: 현재 리포지토리의 코드를 워크플로우 실행 환경으로 가져옵니다.
- **`actions/checkout@v3`**: GitHub에서 제공하는 공식 액션으로, 코드를 체크아웃하는 역할을 합니다. 버전 3을 사용하고 있습니다.

#### 4.2. JDK 설정 (`Set up JDK 21`)

```
- name: Set up JDK 21
  uses: actions/setup-java@v3
  with:
    distribution: 'temurin'
    java-version: '21'
```

- **설명**: Java 개발 키트(JDK) 버전 21을 설정합니다.
- **`actions/setup-java@v3`**: Java 환경을 설정하는 공식 액션입니다.
- **`distribution: 'temurin'`**: Eclipse Temurin 배포판을 사용합니다.
- **`java-version: '21'`**: JDK 21을 설치하고 설정합니다.

#### 4.3. Gradle 캐싱 설정 (`Cache Gradle dependencies`)

```
yaml코드 복사- name: Cache Gradle dependencies
  uses: actions/cache@v3
  with:
    path: |
      ~/.gradle/caches
      ~/.gradle/wrapper/
    key: gradle-cache-${{ runner.os }}-${{ hashFiles('**/*.gradle*', '**/gradle-wrapper.properties') }}
    restore-keys: |
      gradle-cache-${{ runner.os }}-
```

- **설명**: Gradle 빌드 캐시를 설정하여 빌드 시간을 단축시킵니다.

- **`actions/cache@v3`**: GitHub에서 제공하는 캐싱 액션으로, 특정 파일 경로를 캐싱하고 재사용할 수 있게 합니다.

- **`path`**:

  - `~/.gradle/caches`: Gradle 의존성 캐시 경로
  - `~/.gradle/wrapper/`: Gradle 래퍼 파일 경로

- **`key`**:

  - 캐시의 고유 키를 정의합니다.

  - ```
    gradle-cache-${{ runner.os }}-${{ hashFiles('**/*.gradle*', '**/gradle-wrapper.properties') }}
    ```

    :

    - `runner.os`: 실행 환경의 OS (예: Ubuntu)
    - `hashFiles('**/*.gradle*', '**/gradle-wrapper.properties')`: 프로젝트의 `*.gradle` 파일과 `gradle-wrapper.properties` 파일의 해시를 기반으로 캐시 키를 생성합니다.
    - 이를 통해 Gradle 설정이 변경될 때마다 새로운 캐시가 생성됩니다.

- **`restore-keys`**:

  - 기본 키와 일치하지 않을 경우, 부분 일치를 시도할 키를 정의합니다.
  - `gradle-cache-${{ runner.os }}-`: OS별로 캐시를 복원하려고 시도합니다.

**장점**:

- 의존성 다운로드 시간을 줄여 전체 빌드 시간을 단축할 수 있습니다.

  

#### 4.4. Gradlew 실행 권한 부여 (`Make gradlew executable`)

```
yaml코드 복사- name: Make gradlew executable
  run: chmod +x ./gradlew
```

- **설명**: Gradle 래퍼 스크립트(`gradlew`)에 실행 권한을 부여합니다.
- **`run: chmod +x ./gradlew`**: `chmod +x` 명령어를 사용하여 `gradlew` 파일을 실행 가능하게 만듭니다.

**이유**:

- 일부 환경에서는 `gradlew` 스크립트에 실행 권한이 없을 수 있으므로, 이를 명시적으로 설정하여 스크립트를 실행할 수 있게 합니다.

  

#### 4.5. Gradle 빌드 및 테스트 실행 (`Build and Test with Gradle`)

```
yaml코드 복사- name: Build and Test with Gradle
  run: ./gradlew build
```

- **설명**: Gradle을 사용하여 프로젝트를 빌드하고 테스트를 실행합니다.
- **`run: ./gradlew build`**: Gradle 래퍼를 통해 `build` 태스크를 실행합니다. 이는 소스 코드를 컴파일하고 테스트를 수행하며, 결과물을 생성합니다.

------



## 워크플로우의 전체적인 흐름 요약

1. **트리거 이벤트**:
   - **feature 브랜치**에 코드가 푸시(push)될 때
   - **develop 브랜치**로 풀 리퀘스트가 생성되거나 업데이트될 때
   - 워크플로우가 실행됩니다.
2. **작업 환경 설정**:
   - 최신 **Ubuntu** 가상 환경에서 작업을 실행
   - JDK 21 설정 및 Gradle 캐시 구성
3. **단계별 작업**:
   - 코드 체크아웃
   - JDK 21 설치 및 설정
   - Gradle 의존성 캐시 설정
   - Gradle 래퍼 스크립트 실행 권한 부여
   - Gradle 빌드 및 테스트 실행
4. **빌드 및 테스트 결과**:
   - 빌드 및 테스트가 성공하면 PR 상태가 `Success`로 표시됩니다.
   - 실패하면 PR 상태가 `Failed`로 표시되며, 병합이 차단됩니다.



---





## 추가적인 문법 요소 및 팁

### 1. 조건부 실행 (`if`)

특정 조건이 만족될 때만 Job이나 Step을 실행하도록 설정할 수 있습니다.

```
jobs:
  deploy:
    if: github.ref == 'refs/heads/main' && success()
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      - name: Deploy
        run: ./deploy.sh
        env:
          API_KEY: ${{ secrets.API_KEY }}
```

- **설명**: 현재 브랜치가 `main`이고 이전 단계가 성공했을 때만 `deploy` Job이 실행됩니다.

### 2. 매트릭스 전략 (`strategy.matrix`)

여러 환경에서 병렬로 Job을 실행할 때 사용합니다.

```
jobs:
  build:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        java-version: [11, 17, 21]
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      - name: Set up JDK ${{ matrix.java-version }}
        uses: actions/setup-java@v3
        with:
          distribution: 'temurin'
          java-version: ${{ matrix.java-version }}
      - name: Build with Gradle
        run: ./gradlew build
```

- **설명**: `java-version`의 각 값(11, 17, 21)에 대해 별도의 Job이 병렬로 실행됩니다.

### 3. 시크릿 관리 (`secrets`)

민감한 정보를 안전하게 관리하고 사용할 수 있습니다.

```
- name: Deploy
  run: ./deploy.sh
  env:
    API_KEY: ${{ secrets.API_KEY }}
```

- **설명**: GitHub Secrets에 저장된 `API_KEY`를 환경 변수로 사용하여 배포 스크립트에 전달합니다.

### 4. 아티팩트 업로드 및 다운로드

빌드 결과물을 저장하고 다른 Job에서 사용할 수 있습니다.

```
# 빌드 Job에서 아티팩트 업로드
- name: Upload build artifacts
  uses: actions/upload-artifact@v3
  with:
    name: build-artifact
    path: build/libs/

# 배포 Job에서 아티팩트 다운로드
- name: Download build artifacts
  uses: actions/download-artifact@v3
  with:
    name: build-artifact
```

- **설명**: 빌드 결과물을 아티팩트로 업로드하여, 이후 단계에서 다운로드하여 사용할 수 있습니다.

### 5. 캐싱 전략 최적화

Gradle 캐싱을 보다 효율적으로 관리하여 빌드 시간을 단축할 수 있습니다.

```
- name: Cache Gradle dependencies
  uses: actions/cache@v3
  with:
    path: |
      ~/.gradle/caches
      ~/.gradle/wrapper/
    key: gradle-cache-${{ runner.os }}-${{ hashFiles('**/*.gradle*', '**/gradle-wrapper.properties') }}
    restore-keys: |
      gradle-cache-${{ runner.os }}-
```

- **설명**: Gradle 의존성 및 래퍼 파일을 캐싱하여, 다음 빌드 시 빠르게 복원할 수 있습니다.


