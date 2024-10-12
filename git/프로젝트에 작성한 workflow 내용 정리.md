프로젝트에 작성한 workflow 내용 정리

```
# 워크플로의 이름을 정의합니다.
name: Build

# 이벤트 정의: develop 브랜치에 푸시(push)될 때 CI 작업 실행
on:
  push:
    branches:
      - feature  # develop 브랜치에 푸시가 발생할 때 실행
  pull_request:
    branches:
      - develop

# jobs 섹션: 빌드 작업 을 정의합니다.
jobs:
  build:
    runs-on: ubuntu-latest  # Ubuntu 가상 환경에서 실행

# 단계 정의 
 #작업 내에서 순차적으로 실행될 단계들을 정의합니다.
    steps: # 코드 체크아웃, 현재 리포지토리의 코드를 워크플로우 실행환경으로 가져옵니다.
      - name: Check out code
        uses: actions/checkout@v3

      - name: Set up JDK 21 # JDK 설정
        uses: actions/setup-java@v3
        with:
          distribution: 'temurin'
          java-version: '21'

      # Gradle 캐싱 설정
      - name: Gradle Caching
        uses: actions/cache@v3
        with:
          path: |
            ~/.gradle/caches/modules-2/files-2.1
            ~/.gradle/wrapper/
          key: gradle-cache-${{ runner.os }}-${{ hashFiles('**/*.gradle*', '**/gradle-wrapper.properties') }}
          restore-keys: |
            gradle-cache-${{ runner.os }}-
		# Gradlew 실행 권한 부여
      - name: Make gradlew executable
        run: chmod +x ./gradlew
  		# Gradle 빌드 진행
      - name: Build with Gradle
        run: ./gradlew build
```

