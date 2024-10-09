# [실습] Docker로 MySQL 실행시켜보기  - 1

### ✅ Docker로 MySQL 실행시켜보기

1. **MySQL 이미지를 바탕으로 컨테이너 실행시키기**
    
    [mysql - Official Image | Docker Hub](https://hub.docker.com/_/mysql)
    
    ```docker
    $ docker run -e MYSQL_ROOT_PASSWORD=password123 -p 3306:3306 -d mysql
    ```
    
    - **참고)** `docker pull` 과정은 생략해도 상관없다. 왜냐하면 `docker run mysql`로 실행시켰을 때, 로컬에 이미지가 없으면 Dockerhub으로부터 MySQL 이미지를 알아서 다운받아서 실행시키기 때문이다.
    - `-e MYSQL_ROOT_PASSWORD=password123` : `-e` 옵션은 컨테이너의 환경 변수를 설정하는 옵션이다.
    - Dockerhub의 MySQL 공식 문서를 보면 환경 변수로 `MYSQL_ROOT_PASSWORD`를 정해주어야만 정상적으로 컨테이너가 실행된다고 적혀져있따.
    - 아래의 명령어로 컨테이너로 들어가서 환경 변수를 직접 눈으로 확인해보자.
        
        ```bash
        $ docker exec -it [MySQL 컨테이너 ID] bash
        
        $ echo $MYSQL_ROOT_PASSWORD # MYSQL_ROOT_PASSWORD라는 환경변수 값 출력
        $ export # 설정되어 있는 모든 환경변수 출력
        ```
        
    
2. **컨테이너가 잘 실행되고 있는 지 체크**
    
    ```
    $ docker ps
    ```

    ![image](https://github.com/user-attachments/assets/5db5861e-d780-44d6-9967-852a7b89d515)

 
    
3. **컨테이너 실행시킬 때 에러 없이 잘 실행됐는 지 로그 체크**
    
    ```
    $ docker logs [컨테이너 ID 또는 컨테이너명]
    ```
        

### ✅ 그림으로 이해하기

![image](https://github.com/user-attachments/assets/82fe47f2-bf1a-47c7-adfd-e23b74e6fe7f)
