# [실습] Docker Compose로 백엔드(Spring Boot) 실행시키기

#### Docker Compose로 백엔드(Spring Boot) 실행시키기

프로젝트 셋팅

start.spring.io

https://start.spring.io/

![img](https://jscode.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2Fe35a8144-c5ff-40f0-b123-384a331e35bb%2F8ecc70e9-bb6b-4d08-b06b-f860fb575448%2FUntitled.png?table=block&id=9db7629c-18e9-463d-8bdc-7c9af23ff9fa&spaceId=e35a8144-c5ff-40f0-b123-384a331e35bb&width=1360&userId=&cache=v2)


- Java 17 버전을 선택하자. 아래 과정을 Java 17 버전을 기준으로 진행할 예정이다. 

간단한 코드 작성

AppController

Java

```

@RestController
public class AppController {
 @GetMapping("/")
public String home() {
 return "Hello, World!";  }
}

```


Dockerfile 작성하기

Dockerfile
```
Docker


FROM openjdk:17-jdk
COPY build/libs/*SNAPSHOT.jar /app.jar
ENTRYPOINT ["java", "-jar", "/app.jar"]
```

Spring Boot 프로젝트 빌드하기
```
$ ./gradlew clean build
```

compose 파일 작성하기

compose.yml

```

services:
  my-server:
      build: .
      ports:
        - 8080:8080

```

- 'build: .' 의 의미  compose.yml이 존재하는 디렉토리(.)에 있는 Dockerfile로 이미지를 생성해 컨테이너를 띄우겠다는 의미이다. 


compose 파일 실행시키기
```
$ docker compose up -d --build

```
compose 실행 현황 보기
$ docker compose ps
$ docker ps
​```
localhost:8080으로 들어가보기


compose로 실행된 컨테이너 삭제
```
$ docker compose down
```

