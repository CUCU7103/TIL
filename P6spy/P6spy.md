# P6spy란?
> JPA에 대한 로그를 보다보면 where 절에 사용되는 파라미터 부분에 "?" 라고 찍히는 것을 확인할 수 있다. <br>
> 일반적인 hibarnate 로깅 설정으로는 이를 확인할 수없다.<br>
> 이 문제를 해결하기 위해 사용할 수 있는 라이브러리가 P6spy이다.

<br>

## 💡 정의

- ### P6Spy <br>

  - 자바 애플리케이션에서 JDBC(Java Database Connectivity)를 모니터링하고 디버깅하는 데 사용되는 툴입니다.
  - 주로 데이터베이스 쿼리의 실행 시간, 호출된 스토어드 프로시저, 연결 및 트랜잭션 정보를 기록하는 데 사용됩니다.
  - 일반적으로 P6Spy는 JDBC 드라이버를 래핑하여 애플리케이션이 데이터베이스와 상호 작용할 때의 성능 및 동작을 추적할 수 있도록 도와줍니다.
  - 이는 개발자가 애플리케이션의 데이터베이스 상호 작용을 디버깅하거나 최적화하는 데 도움이 됩니다.


## 💡 스프링부트 3.x.x 버전에서의 사용법
 - bulid.gradle
  ```java 
    //  build Gradle에 해당 의존성 추가
    implementation 'com.github.gavlyukovskiy:p6spy-spring-boot-starter:1.9.0'
  ```
  - application.yml

```java
spring:
  datasource:
    url: jdbc:mariadb://localhost:3306/your_database
    username: your_username
    password: your_password
    driver-class-name: org.mariadb.jdbc.Driver
    
# P6spy 사용하는데 필요
decorator:
  datasource:
    p6spy:
      enable-logging: true
      multiline: true
      logging: slf4j
       tracing:
        include-parameter-values: true

logging:
  level:
    p6spy: info
```
