# Docker를 통한 MSSQL 설치

### MSSQL 이미지 파일을 통한 도커 설치

- 이미지 파일을 받습니다.
-  ``` docker pull mcr.microsoft.com/mssql/server:2022-latest```

- 이미지 파일을 통해서 MSSQL 컨테이너를 실행. <p>
```
  docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<당신의@비밀번호입력>" `
   -p 1433:1433 --name sql1 --hostname sql1 `
   -d `
   mcr.microsoft.com/mssql/server:2022-latest
```

- 정상적으로 실행되었는지 확인하기 <p>
```
docker ps

```
![image](https://github.com/user-attachments/assets/8e4cac85-0963-400d-b580-72d2bdf73ee3)

- Docker container에 접속하기 <p>
```
  docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P "admin@Qwer!234"
```

- 해당 명령을 통해서 접속이 안되고 아래와 같은 오류가 발생하는 상황이 있을 수 있다.


```
  OCI runtime exec failed: exec failed: unable to start container process: exec: 
  "/opt/mssql-tools/bin/sqlcmd": stat /opt/mssql-tools/bin/sqlcmd: no such file or directory: unknown
```

gpt 설명
- 해당 오류는 sqlcmd 명령어가 컨테이너 내에서 설치되어 있지 않기 때문에 발생하는 문제입니다.<br>
- mcr.microsoft.com/mssql/server:2022-latest 이미지에는 sqlcmd 도구가 기본적으로 포함되어 있지 않을 수 있습니다

1. 컨테이너에 접속하기

먼저, bash 셸로 컨테이너에 접속합니다.
```
bash
docker exec -it sql1 /bin/bash
```

2. sqlcmd 도구 설치

컨테이너 내부에서 sqlcmd 도구를 설치하려면, 아래 명령어를 실행하세요.

```
bash
apt-get update
apt-get install -y mssql-tools unixodbc-dev
```

3. sqlcmd 도구로 접속
   
이제 sqlcmd 도구를 이용해 SQL Server에 접속할 수 있습니다.

```
bash
/opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P "admin@Qwer!234"
```

sqlcmd 설치가 완료되었으면 아래 명령어를 실행하여 컨테이너 실행
```
docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P "admin@Qwer!234"
```

- 계정 설정을 하는게 정석이나 일단 테스트를 위해 sa 계정사용
- 데이터베이스 생성
  - create database db명;
  - GO 
