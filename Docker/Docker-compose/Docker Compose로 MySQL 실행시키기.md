# [실습] Docker Compose로 MySQL 실행시키기

### ✅ Docker CLI로 컨테이너를 실행시킬 때

```html
$ docker run -e MYSQL_ROOT_PASSWORD=pwd1234 -p 3306:3306 -v /Users/jaeseong/Documents/Develop/docker-mysql/mysql_data:/var/lib/mysql -d mysql
```

### ✅ Docker Compose로 MySQL 실행시키기

1. **compose 파일 작성하기**

   **compose.yml**

   ```bash
   services:
     my-db:
       image: mysql
       environment:
         MYSQL_ROOT_PASSWORD: pwd1234
       volumes:
         - ./mysql_data:/var/lib/mysql
       ports:
         - 3306:3306
   ```

   - `environment: ...` : CLI에서 `-e MYSQL_ROOT_PASSWORD=password` 역할과 동일하다.
   - `volumes: ...` : CLI에서 `-v {호스트 경로}:/var/lib/mysql` 역할과 동일하다.

2. **compose 파일 실행시키기**

   ```bash
   $ docker compose up -d
   ```

3. **compose 실행 현황 보기**

   ```bash
   $ docker compose ps
   $ docker ps
   ```

4. **잘 작동하는 지 DBeaver에 연결시켜보기**

  ![image](https://github.com/user-attachments/assets/2edd0d2d-f373-44cf-a4cd-68a8efc81f73)

5. **volume의 경로에 데이터가 저장되고 있는 지 확인하기**

6. **compose로 실행된 컨테이너 삭제**
