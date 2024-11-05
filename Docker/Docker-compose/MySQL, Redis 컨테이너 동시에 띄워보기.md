# [실습] MySQL, Redis 컨테이너 동시에 띄워보기



### Docker Compose로 MySQL, Redis 실행시키기

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

  my-cache-server:
    image: redis
    ports:
      - 6379:637
```

- 주의) YAML 문법에서는 들여쓰기가 중요하다. 



compose 파일 실행시키기

```
$ docker compose up -d
```



compose 실행 현황 보기

```
$ docker compose ps
$ docker ps
```



compose로 실행된 컨테이너 삭제

```
$ docker compose down
```

