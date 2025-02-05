# spring-session-data-redis

- Spring Session에서 제공하는 Redis 기반의 세션 저장소입니다.

- Spring Session 이란?

  - **Spring Session**은 애플리케이션의 세션을 좀 더 체계적이고 유연하게 관리하기 위해 제공되는 **Spring 프로젝트**입니다. 

  - **Spring Session**은 **사용자의 세션 정보를 관리하기 위한 API와 구현을 제공**합니다.

  - 간단히 말하면, **사용자의 세션(로그인 정보, 장바구니 정보 등)을 서버 메모리 대신 외부 저장소(예: Redis, JDBC, MongoDB 등)에 저장하고 관리**할 수 있도록 도와줍니다 

  - 공식 문서에서 Spring session은 다음과 같은 모듈로 구성되어져 있다고 나옵니다.

    ![image-20250205235719596](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20250205235719596.png)

  

- 즉 위의 내용을 통해 알 수있듯 **Spring Session은 세션 정보를 다양한 방식으로 관리하기 위한 추상화 계층을 제공하며 그 구현체 중 하나인 Spring Session Data Redis는 Redis를 사용해 Spring의 세션 정보를 저장하도록 돕습니다**.
  


-- 참고한 블로그
https://data-make.tistory.com/780
https://ksh-coding.tistory.com/128
