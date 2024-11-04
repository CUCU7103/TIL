# [실습] Docker Compose로 백엔드(Nest.js) 실행시키기

#### Docker Compose로 백엔드(Nest.js) 실행시키기

Nest.js 프로젝트 만들기

```
# Nest CLI 설치
$ npm i -g @nestjs/cli

# nest new {프로젝트명}
$ nest new my-server
```





Dockerfile 작성하기

```
FROM node

WORKDIR /app

COPY . .

RUN npm install

RUN npm run build

EXPOSE 3000

ENTRYPOINT [ "node", "dist/main.js" ]
```





.dockerignore 작성하기

```
node_modules
```

이미지를 생성할 때 npm install을 통해 처음부터 깔끔하게 필요한 의존성만 설치한다. 따라서 호스트 컴퓨터에 있는 node_modules는 컨테이너로 복사해갈 필요가 없다. 





compose 파일 작성하기

참고) compose를 작성하지 않고 Docker CLI로 실행시킬 때

```
$ docker build -t my-server .
$ docker run -d -p 3000:3000 my-server
```



compose.yml

```
services:
  my-server:
    build: .
    ports:
      - 3000:3000
```



compose 파일 실행시키기

```
$ docker compose up -d --build
```





compose 실행 현황 보기

```
$ docker compose ps
$ docker ps
```





localhost:3000으로 들어가보기

![image](https://github.com/user-attachments/assets/b76763ad-0150-4650-a725-729960c7f93c)



compose로 실행된 컨테이너 삭제

```
$ docker compose down
```

