
# 자주 사용하는 Docker Compose CLI 명령어

> [!TIP]
>
> - #### `docker-compose`로 시작하는 명령어는 더 이상 업데이트를 지원하지 않는 Docker Compose의 v1 명령어이므로 되도록이면 사용하지 말자. 
>
> - #### v2부터는 `docker compose`로 시작하는 명령어를 사용한다.



> [!IMPORTANT]
>
> #### 아래 명령어들은 `compose.yml`이 존재하는 디렉토리에서 실행시켜야 한다.

### ✅ compose 파일 작성

**compose.yml**

```bash
services:
	websever:
		container_name: webserver
		image: nginx
		ports: 
			- 80:80
```



### ✅ compose.yml에서 정의한 컨테이너 실행

```bash
$ docker compose up    # 포그라운드에서 실행
$ docker compose up -d # 백그라운드에서 실행
```

- `-d` : 백그라운드에서 실행

  

### ✅ Docker Compose로 실행시킨 컨테이너 확인하기

```bash
# compose.yml에 정의된 컨테이너 중 실행 중인 컨테이너만 보여준다. 
$ docker compose ps 

# compose.yml에 정의된 모든 컨테이너를 보여준다.
$ docker compose ps -a
```

### ✅ Docker Compose 로그 확인하기

```bash
# compose.yml에 정의된 모든 컨테이너의 로그를 모아서 출력한다.
$ docker compose logs
```

### ✅ 컨테이너를 실행하기 전에 이미지 재빌드하기

```bash
$ docker compose up --build # 포그라운드에서 실행
$ docker compose up --build -d # 백그라운드에서 실행
```

- `compose.yml`에서 정의한 이미지 파일에서 코드가 변경 됐을 경우, 이미지를 다시 빌드해서 컨테이너를 실행시켜야 코드 변경된 부분이 적용된다. 
- 그러므로 이럴 때에는 `--build` 옵션을 추가해서 사용해야 한다.



> [!NOTE]
>
> **참고** : `docker compose up` vs `docker compose up --build`
>
> - `docker compose up` : 
>   - 이미지가 없을 때만 빌드해서 컨테이너를 실행시킨다. 
>   - 이미지가 이미 존재하는 경우 이미지를 빌드하지 않고 컨테이너를 실행시킨다.
> - `docker compose up --build` : 
>   - 이미지가 있건 없건 무조건 빌드를 다시해서 컨테이너를 실행시킨다.



### ✅ 이미지 다운받기 / 업데이트하기

```bash
$ docker compose pull
```

- ```
  compose.yml
  ```

  에서 정의된 이미지를 다운 받거나 업데이트 한다.

  - 로컬 환경에 이미지가 없다면 이미지를 다운 받는다.

  - 로컬 환경에 이미 이미지가 있는데, Dockerhub의 이미지와 다른 이미지일 경우 이미지를 업데이트 한다.

    

### ✅ Docker Compose에서 이용한 컨테이너 종료하기

```bash
$ docker compose down
```







