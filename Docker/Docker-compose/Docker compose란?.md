# Docker-compose

- ## **docker compose란?**

  - 여러개의 docker 컨테이너들을 하나의 서비스로 정의하고 구성해 하나의 묶음으로 관리할 수 있게 도와주는 툴 입니다.



- ## 사용하는 이유

  - ### **여러개의 컨테이너를 관리하는데 유용합니다.**

    - 여러 개의 컨테이너로 이루어진 복잡한 애플리케이션을 한 번에 관리할 수 있게 해줍니다.
    - 여러 컨테이너를 하나의 환경에서 실행하고 관리하는데 도움이 됩니다.

  - ### **복잡한 명령어로 실행시키던걸 간소화 할 수있습니다.**

    - 이전에 MySQL 이미지를 컨테이너로 실행시킬 때 아래와 같은 명령어를 실행시켰다.

      ```bash
      $ docker run -e MYSQL_ROOT_PASSWORD=password123 -p 3306:3306 -v /Users/jaeseong/Documents/Develop/docker-mysql/mysql_data:/var/lib/mysql -d mysql
      ```

      Docker Compose를 사용하면 위와 같이 컨테이너를 실행시킬 때마다 복잡한 명령어를 입력하지 않아도 됩니다.

      단순히 `docker compose up` 명령어만 실행시키면 된다.



- ### **사용단계 (거시적인 시점)**

  1. Docker Compose 파일 작성: YAML 형식으로 작성된 Docker Compose 파일을 사용해 서비스와 관련 설정들을 정의.
  2. Docker Compose 실행: docker-compose up 명령어를 사용하여 Docker Compose 파일에 정의된 서비스들을 시작.
  3. 실행 및 관리: 실행된 서비스를 확인하고 관리하기 위해 docker-compose ps, docker-compose logs, docker-compose exec 등의 명령어를 사용한다.
  4. 중지 및 정리: 작업을 완료한 후 docker-compose down 명령어를 사용하여 Docker Compose 파일에 정의된 서비스들을 중지하고 관련 컨테이너들을 삭제.
