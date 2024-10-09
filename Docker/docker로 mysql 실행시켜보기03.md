- # [실습] Docker로 MySQL 실행시켜보기 - 3

  ### ✅ 볼륨(Volume)을 활용해 MySQL 컨테이너 띄우기

  1. **MySQL 컨테이너 띄우기**

     ```bash
     $ cd /Users/jaeseong/Documents/Develop
     $ mkdir docker-mysql # MySQL 데이터를 저장하고 싶은 폴더 만들기
     
     # docker run -e MYSQL_ROOT_PASSWORD=password123 -p 3306:3306 **-v {호스트의 절대경로}/mysql_data:/var/lib/mysql** -d mysql
     
     $ docker run -e MYSQL_ROOT_PASSWORD=password123 -p 3306:3306 -v /Users/jaeseong/Documents/Develop/docker-mysql/mysql_data:/var/lib/mysql -d mysql
     ```

  - `pwd` 명령어로 볼륨으로 사용하고자 하는 경로를 확인한 뒤 입력해주자.

    > **주의)** `mysql_data` 디렉토리를 미리 만들어 놓으면 안 된다. 
    >
    > 그래야 처음 이미지를 실행시킬 때 mysql 내부에 있는 `/var/lib/mysql` 파일들을 호스트 컴퓨터로 공유받을 수 있다.
    >
    >  `mysql_data` 디렉토리를 미리 만들어놓을 경우, 기존 컨테이너의 `/var/lib/mysql` 파일들을 전부 삭제한 뒤에 `mysql_data`로 덮어씌워 버린다.

  - DB에 관련된 데이터가 저장되는 곳이 `/var/lib/mysql`인지는 Dockerhub MySQL의 공식 문서에 나와있다.

    ![img](https://jscode.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2Fe35a8144-c5ff-40f0-b123-384a331e35bb%2F9bb1fc1b-7451-46d6-97ec-925da37b4894%2FUntitled.png?table=block&id=82488e4f-eac7-47d7-bf5f-5c4b19cd7ea1&spaceId=e35a8144-c5ff-40f0-b123-384a331e35bb&width=2000&userId=&cache=v2)

  1. **MySQL 컨테이너에 접속해서 데이터베이스 만들기**

     ```bash
     $ docker exec -it [MySQL 컨테이너 ID] bash
     
     $ mysql -u root -p
     
     mysql> show databases;
     mysql> create database mydb;
     mysql> show databases;
     ```

  2. **컨테이너 종료 후 다시 생성해보기**

     ```bash
     # 컨테이너 종료
     $ docker stop [MySQL 컨테이너 ID]
     $ docker rm [MySQL 컨테이너 ID]
     
     # 컨테이너 생성
     $ docker run -e MYSQL_ROOT_PASSWORD=password123 -p 3306:3306 -v /Users/jaeseong/Documents/Develop/docker-mysql/mysql_data:/var/lib/mysql -d mysql
     
     $ docker exec -it [MySQL 컨테이너 ID] bash
     $ mysql -u root -p
     mysql> show databases; # 아까 생성한 데이터베이스가 그대로 존재하는 걸 확인할 수 있다.
     ```

     - volume을 통해서 mysql 데이터를 컨테이너가 아니라 호스트의 저장공간에 따로 빼서 저장해놓는다.

  

  ### ✅ 그림으로 이해하기

  ![img](https://jscode.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2Fe35a8144-c5ff-40f0-b123-384a331e35bb%2F4785d2ca-55b6-4608-83fb-784271a0d1f1%2FUntitled.png?table=block&id=8a807ee2-4f8e-4eae-a7e6-dad81e6a9821&spaceId=e35a8144-c5ff-40f0-b123-384a331e35bb&width=2000&userId=&cache=v2)

  ### ✅ MySQL 컨테이너 삭제하고 다시 띄워보기

  ```bash
  # 컨테이너 종료
  $ docker stop [MySQL 컨테이너 ID]
  $ docker rm [MySQL 컨테이너 ID]
  
  # 비밀번호 바꿔서 컨테이너 생성
  $ docker run -e MYSQL_ROOT_PASSWORD=**pwd1234** -p 3306:3306 -v /Users/jaeseong/Documents/Develop/docker-mysql/mysql_data:/var/lib/mysql -d mysql
  
  $ docker exec -it [MySQL 컨테이너 ID] bash
  $ mysql -u root -p # 접속이 안 됨...
  ```

  > 분명 `MYSQL_ROOT_PASSWORD` 값을 바꿔서 새로 컨테이너를 띄웠는데 비밀번호는 바뀌지 않은걸까? 이 부분 때문에 많은 분들이 헤맨다.
  >
  > 그 이유는 Volume으로 설정해둔 폴더에 이미 비밀번호 정보가 저장되버렸기 때문이다.









