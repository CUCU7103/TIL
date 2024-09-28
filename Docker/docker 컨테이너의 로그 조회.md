# 컨테이너(Container) 로그 조회

<aside>
👨🏻‍🏫 컨테이너를 실행시키고나서 실행시킨 컨테이너가 잘 실행되고 있는 지, 에러가 발생한 건 아닌 지 로그를 확인할 수 있어야 한다. 
  디버깅할 때 필수로 확인해야 하는 게 로그다. 
  지금부터 컨테이너에서 발생한 로그는 어떻게 확인하는 지 알아보자.
</aside>

### ✅ 컨테이너(Container) 로그 조회

**[특정 컨테이너의 모든 로그 조회]**

```bash
# docker logs [컨테이너 ID 또는 컨테이너명]

$ docker run -d nginx
$ docker logs [nginx가 실행되고 있는 컨테이너 ID]
```

**[최근 로그 10줄만 조회]**

```bash
# dokcer logs --tail [로그 끝부터 표시할 줄 수] [컨테이너 ID 또는 컨테이너명]
$ dokcer logs --tail  [컨테이너 ID 또는 컨테이너명]
```

**[기존 로그 조회 + 생성되는 로그를 실시간으로 보고 싶은 경우]**

```bash
# docker logs -f [컨테이너 ID 또는 컨테이너명]

# Nginx의 컨테이너에 실시간으로 쌓이는 로그 확인하기
$ docker run -d -p 80:80 nginx
$ docker logs -f
```

- `-f` : follow의 약어

**[기존 로그는 조회하지 않기 + 생성되는 로그를 실시간으로 보고 싶은 경우]**

```
$ docker logs --tail 0 -f [컨테이너 ID 또는 컨테이너명]
```
