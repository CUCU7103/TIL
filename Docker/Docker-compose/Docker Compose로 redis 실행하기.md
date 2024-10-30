# [실습] Docker Compose로 Redis 실행시키기

### ✅ Docker CLI로 컨테이너를 실행시킬 때

```html
$ docker run -d -p 6379:6379 redis
```

### ✅ Docker Compose로 컨테이너를 실행시킬 때

1. **compose.yml 파일 작성하기**

   **compose.yml**

   ```bash
   services:
   	my-cache-server:
   		image: redis
   		ports: 
   			- 6379:6379 
   ```

2. **compose 파일 실행시키기**

   ```bash
   $ docker compose up -d
   ```

3. **compose 실행 현황 보기**

   ```bash
   $ docker compose ps
   $ docker ps
   ```

4. **컨테이너 실행시킬 때 에러 없이 잘 실행됐는 지 로그 체크**

   ```
   $ docker logs [컨테이너 ID 또는 컨테이너명]
   ```

5. **Redis 컨테이너에 접속**

   ```
   $ docker exec -it [컨테이너 ID 또는 컨테이너명] bash
   ```

6. **컨테이너에서 redis 사용해보기**

   ```
   $ redis-cli
   
   127.0.0.1:6379> set 1 jscode
   127.0.0.1:6379> get 1
   ```

   ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8b0031e1-3bbf-49bc-9b38-272a139d6839/Untitled.png)

   ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/e35a8144-c5ff-40f0-b123-384a331e35bb/e4872a18-efa9-4a43-85fb-a59928365379/Untitled.png)

7. **compose로 실행된 컨테이너 삭제**

   ```bash
   $ docker compose down
   ```
