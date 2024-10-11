- 아래와 같이 도커 명령어를 실행한다고 했을때 c/volume_test/docker-mysql/mysql_data 이 부분에 이미 파일이 존재한다면?

``` ash
docker run -e MYSQL_ROOT_PASSWORD=password123 -p 3400:3306 -v /c/volume_test/docker-mysql/mysql_data:/var/lib/mysql -d mysql
```

컨테이너는 정상적으로 실행되지 않는다. 

왜냐하면 

```
$ docker run -v [호스트의 디렉토리 절대경로]:[컨테이너의 디렉토리 절대경로] [이미지명]:[태그명]
```

- `[호스트의 디렉토리 절대 경로]`에 디렉토리가 이미 존재할 경우, 호스트의 디렉터리가 컨테이너의 디렉터리를 덮어씌우기 때문입니다.

- 이미 경로가 존재하기에 그 안에있는 데이터를 컨테이너의 디렉터리에 덮어씌워서 오류가 발생합니다, 즉 정상적으로 컨테이너 초기화가 되지 않습니다.

  ![img](https://jscode.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2Fe35a8144-c5ff-40f0-b123-384a331e35bb%2Fd7a7ae84-0710-4a29-b534-74cb91d3870b%2FUntitled.png?table=block&id=cd74c1f7-6e55-42b0-8224-7849c0b779b0&spaceId=e35a8144-c5ff-40f0-b123-384a331e35bb&width=1360&userId=&cache=v2)

  

- `[호스트의 디렉토리 절대 경로]`에 디렉토리가 존재하지 않을 경우, 호스트의 디렉터리 절대 경로에 디렉터리를 새로 만들고 컨테이너의 디렉터리에 있는 파일들을 호스트의 디렉터리로 복사해옵니다.

  ![img](https://jscode.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2Fe35a8144-c5ff-40f0-b123-384a331e35bb%2F648c95d0-2402-4b6f-b1d9-e8b5395fd602%2FUntitled.png?table=block&id=69e8a355-82bb-435c-8177-2018990d8199&spaceId=e35a8144-c5ff-40f0-b123-384a331e35bb&width=1360&userId=&cache=v2)

- volume 경로를 미리 지정하고 사용하는것과 그렇지 않은 방식을 사용하는 것에는 위와같은 차이점이 존재하니 알맞게 사용해야 합니다.











