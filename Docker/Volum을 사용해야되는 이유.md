# [실습] Docker로 MySQL 실행시켜보기 - 2

### ✅ MySQL 컨테이너에 직접 접속해보기

1. **MySQL 컨테이너에 접속**
    
    ```
    $ docker exec -it [MySQL 컨테이너 ID] bash
    ```
    

    
2. **컨테이너에서 MySQL에 접근하기**
    
    ```
    $ mysql -u root -p
    ```
    

    
3. **MySQL 접근에 성공했다면 데이터베이스 조회해보기**
    
    ```
    mysql> show databases;
    ```
    
    

4. **데이터베이스 만들기**
    
    ```bash
    mysql> create database mydb;
    mysql> show databases;
    ```
    

5. **컨테이너 종료 후 다시 생성해보기**
    
    ```bash
    # 컨테이너 종료
    $ docker stop [MySQL 컨테이너 ID]
    $ docker rm [MySQL 컨테이너 ID]
    
    # 컨테이너 생성
    $ docker run -e MYSQL_ROOT_PASSWORD=password123 -p 3306:3306 -d mysql
    $ docker exec -it [MySQL 컨테이너 ID] bash
    
    $ mysql -u root -p
    mysql> show databases; # 아까 생성한 데이터베이스가 없어진 걸 확인할 수 있다. 
    ```
    

<aside>
👨🏻‍🏫 위 방식은 볼륨(Volume)을 활용하지 않고 MySQL 컨테이너를 띄웠다. 그래서 MySQL 컨테이너를 삭제함과 동시에 MySQL 내부에 저장되어 있던 데이터도 함께 삭제되어 없어지는 상황이 발생하였습니다. 

</aside>
