# chapter 14-02



## 주요 sql 타입

| SQL 타입     | 설명                                                         |
| ------------ | ------------------------------------------------------------ |
| CHAR         | 확정 길이의 문자열을 저장한다. 표준의 경우 255글자까지만 저장할 수 있다. |
| VARCHAR      | 가변 길이의 문자열을 저장한다. 표준의 경우 255글자까지만 저장할 수 있다. |
| LONG VARCHAR | 긴 가변 길이의 문자열을 저장한다.                            |
| NUMERIC      | 숫자를 저장한다.                                             |
| DECIMAL      | 십진수를 저장한다.                                           |
| INTEGER      | 정수를 저장한다.                                             |
| TIMESTAMP    | 날짜와 시간을 저장한다.                                      |
| TIME         | 시간을 저장한다.                                             |
| DATE         | 날짜를 저장한다.                                             |
| CLOB         | 대량의 문자열 데이터를 저장한다.                             |
| BLOB         | 대량의 이진 데이터를 저장한다                                |



## 테이블 생성 쿼리

```sql
CREATE TABLE db명.테이블명 (
  컬럼명1 INT PRIMARY KEY AUTO_INCREMENT, -- 기본키 숫자 자동 증가 설정
  컬럼명2 CHAR(15) NOT NULL,
  컬럼명3 INT,

  PRIMARY KEY(컬럼명1),
  FOREIGN KEY(컬럼명2) REFERENCES 테이블명(컬럼명) -- 자기자신 외래키 참조
  FOREIGN KEY(컬럼명3) REFERENCES 다른테이블명(컬럼명a) -- 다른테이블 외래키 참조
);
```

- NOT NULL : 해당 필드는 NULL 값을 저장할 수 없게 됩니다.
- UNIQUE : 해당 필드는 서로 다른 값을 가져야만 합니다.
- PRIMARY KEY : 해당 필드가 NOT NULL과 UNIQUE 제약 조건의 특징을 모두 가지게 됩니다.
- FOREIGN KEY : 하나의 테이블을 다른 테이블에 의존하게 만듭니다.
- DEFAULT : 해당 필드의 기본값을 설정합니다.
- AUTO_INCREMENT : 해당 필드의 값을 1부터 시작하여 새로운 레코드가 추가될 때마다 1씩 증가된 값을 저장합니다

## 테이블 데이터 조회 쿼리

- 작성 순서

``` sql
SELECT 컬럼명, 집계함수 as 별명   ----------------- (5)
FROM 테이블명                     ----------------- (1)
WHERE 테이블 조건                 ----------------- (2)
GROUP BY 컬럼명                   ----------------- (3)
HAVING 그룹 조건                  ----------------- (4)
ORDER BY 컬럼명                   ----------------- (6)
```

1. **FROM** : 
   - SQL은 구문이 들어오면 테이블을 가장 먼저 확인합니다. 
2. **WHERE** : 
   - 테이블명을 확인했으니, 테이블에서 주어진 조건에 맞는 데이터들을 추출해줍니다.
3. **GROUP BY** : 
   - 조건에 맞는 데이터가 추출되었으니, 공통적인 데이터들끼리 묶어 그룹을 만들어줍니다.
4. **HAVING** : 
   - 공통적인 데이터들이 묶여진 그룹 중, 주어진 주건에 맞는 그룹들을 추출해줍니다.
5. **SELECT** : 
   - 최종적으로 추출된 데이터들을 (또 함수로 묶어 계산결과를) 조회합니다.
6. **ORDER BY** : 
   - 추출된 데이터들을 정렬해줍니다.



- 실행순서

```sql
select 필드1, 필드2, sum(필드명) as 별명
from 테이블명
where 필드명1=값 (조건)
group by 필드명1
having 별명 > 100 (그룹 조건)
order by 필드명2 desc (정렬)
```

5. select 
   - 필드(열)들과 집계함수 결과값을 선택하고 as 로 별명을 지정

1. from 
   - 어느 테이블에서

2. where 어느 조건을 만족하는 것만
   -  (select문에 집계함수가 있든없든 무조건 where조건부터 맞추고 집계한다.)

3. group 
   - 그룹핑

4. having 
   - 집계용 조건

6. order 
   - 정렬



## 테이블 구조 수정 

### **필드 추가 (add)**

```sql
ALTER TABLE 테이블이름 ADD 필드이름 필드타입

-- 컬럼을 추가하는데 어느 필드 after 이후에 추가하는지 위치를 지정해 줄 수 있다.
ALTER TABLE 테이블이름 ADD 필드이름 필드타입 AFTER 기존필드명
```

### 필드 제거 (drop)

```sql
ALTER TABLE 테이블이름 DROP 필드이름Copy
```

### 필드 수정(change)

```sql
ALTER TABLE 테이블이름 change 필드명 새필드명 새필드타입
```

### 키 추가 (add ...key)

``` sql
ALTER TABLE 테이블이름 add constraint 기본키명 primary key (필드값)

ALTER table 테이블이름 add FOREIGN KEY(columnName) REFERENCES 참조테이블(참조컬럼);
```

### 키 제거

```sql
ALTER TABLE 테이블이름 drop foreign key 외래키명
```

### 테이블 이름 변경

``` sql
ALTER TABLE table_name1 RENAME table_name2;
```



## 테이블 데이터 수정하기

```sql
UPDATE 테이블이름
SET 필드이름1=데이터값1, 필드이름2=데이터값2, ...
WHERE 필드이름=데이터값; -- 조건식을 안쓰면 테이블 전체 레코드가 싹 바뀜
```

## 테이블 데이터 삽입하기

``` sql
-- 필드 몇개만 정하여 넣을 때
INSERT INTO 테이블이름(필드이름1, 필드이름2, 필드이름3, ...)
VALUES (데이터값1, 데이터값2, 데이터값3, ...)

-- 필드 전체를 넣을때 (필드명 생략 가능)
INSERT INTO 테이블이름
VALUES (데이터값1, 데이터값2, 데이터값3, ...)
```



## 테이블 삭제하기

``` sql
drop table 테이블명

DROP DATABASE IF EXISTS Hotel; -- 에러 방지를 위해 if문 추가
DROP TABLE IF EXISTS Reservation;
```



## 테이블 데이터 삭제하기 

![image](https://github.com/user-attachments/assets/7d0ef408-f8ae-490c-818a-fd12dc275a93)



### Delete

- 트랜잭션 로그를 기록해서 **속도가 느림**.
- 지운거 **복구가능**.
- 테이블 자체 용량은 안줄어들음. **휴지통 개념**.

```sql
DELETE FROM 테이블이름
WHERE 필드이름=데이터값
```



### TRUNCATE

- 테이블 구조는 남기고 데이터값만 삭제, 
- **복구 불가**

```sql
truncate table 테이블명
```





# JDBC (Java Database Connectivity)란?

- JDBC(Java Database Connectivity)는 **자바 프로그래밍 언어를 사용해 데이터베이스에 접근할 수 있도록 하는 자바 API이다.**

- 이를 통해서 데이터베이스에 접속하고, SQL을 실행하고, 데이터를 가져오거나 삭제하는 등 데이터를 다룰 수 있게 됩니다.

  ![image](https://github.com/user-attachments/assets/a5c1ef21-5679-4dbc-b160-6791f0e58a3e)


### JDBC에서의 트랜잭션

- 트랜잭션은 DB의 상태를 변경시키기 위해 수행하는 작업 단위입니다.

- 트랜잭션은 작업의 완전성 을 보장해줍니다.

  - 논리적인 작업 셋을 모두 완벽하게 처리하거나 또는 처리하지 못할 경우에는 원 상태로 복구해서 작업의 일부만 적용되는 현상이 발생하지 않게 만들어주는 기능입니다.

  

### JDBC의 동작과정

- 먼저 JDBC API를 사용하기 위해서는 JDBC 드라이버를 먼저 로딩한 후 데이터베이스와 연결해야 한다.
  - JDBC 드라이버는 **JDBC 인터페이스를 구현한 구현체라**고 생각할 수 있으며
    **특정 데이터베이스 벤더(Oracle, MySQL, PostgreSQL 등)에 대한 연결과 데이터베이스에 대한 작업을 가능하게 해준다**.(JDBC 드라이버의 구현체를 이용해서 특정 벤더의 데이터베이스에 접근할 수 있음)
  - 즉 jdbc 드라이버는 데이터베이스와 직접적인 통신을 관리한다.
- JDBC가 제공하는 **DriverManager가 드라이버들을 관리하고** Connection을 획득하는 기능을 제공한다.
- 이 획득한 Connection을 통해서 데이터베이스에 SQL을 실행하고 결과를 응답받을 수 있다.

![image](https://github.com/user-attachments/assets/c60e8ef0-8485-489e-bb2a-742025452a5b)


```java
import java.sql.*;

public class Jdbc {
    public static void main(String[] args) {
        // 0. 데이터베이스 연결 정보 설정
        String url = "jdbc:mysql://localhost:3306/mydatabase";
        String username = "myusername";
        String password = "mypassword";

        Connection connection = null;
        Statement statement = null;
        ResultSet resultSet = null;

        try {
            // 1. DriverManger를 통해 Connection 획득
            // 드라이버 로딩 진행
            // Connection 객체 생성함
            connection = DriverManager.getConnection(url, username, password);

            // 2. Connection을 통해 Statement 생성
            // Statement 객체 생성함.
            statement = connection.createStatement();
            String sql = "SELECT * FROM users";

            // 3. 질의 수행 및 Statement를 통해 ResultSet 생성
            resultSet = statement.executeQuery(sql);

            // 4. ResultSet을 통해 데이터 획득
            while (resultSet.next()) {
                int id = resultSet.getInt("id");
                String name = resultSet.getString("name");
                int age = resultSet.getInt("age");

                System.out.println("ID: " + id + ", Name: " + name + ", Age: " + age);
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            // 5. 리소스 정리
            // 열어준 것에 역순으로 닫아준다.
            try {
                if (resultSet != null) {
                    resultSet.close(); // result close
                }
                if (statement != null) {
                    statement.close(); // stament  close
                }
                if (connection != null) {
                    connection.close(); // connection close
                }
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```

진행 순서

![image](https://github.com/user-attachments/assets/a681ffbc-939f-4290-9d76-e3837c97b479)




### 커넥션 풀

- 커넥션이란?
  - 데이터베이스와 연결되는 세션(Session)입니다.
  - 애플리케이션과 데이터베이스 서버 간의 통신 링크를 의미합니다.

- 커넥션을 생성하는 과정
  - 애플리케이션에서 DB 드라이버를 통해 커넥션을 조회한다.
  - DB 드라이버는 DB와 TCP/IP 커넥션을 연결한다.
  - DB 드라이버는 TCP/IP 커넥션이 연결되면 아이디와 패스워드, 기타 부가 정보를 DB에 전달한다.
  - DB는 아이디, 패스워드를 통해 내부 인증을 거친 후 내부에 DB를 생성한다.
  - DB는 커넥션 생성이 완료되었다는 응답을 보낸다.
  - DB 드라이버는 커넥션을 생성해서 클라이언트에 반환한다
  - 커낵션 사용 후 해당 커넥션을 종료한다.

![image](https://github.com/user-attachments/assets/20df4a2d-cf12-45bd-b8f3-e07c8398076f)


- 매번 사용할때마다 커넥션을 생성하는 것은 네트워크와 서버의 자원을 매번 사용해야 합니다.

  - 이는 리소스의 낭비이자 비효율적입니다.

  - 이러한 문제점을 해결하기 위해  커넥션 풀을 사용합니다.



## 커넥션 풀이란? (Connection pool)

![image](https://github.com/user-attachments/assets/b49f989b-d7b5-42ed-810a-7cbc16646019)


- **Connection를 미리 생성하여 보관**하고 애플리케이션이 필요할 때 꺼내서 사용할 수 있도록 관리해 주는 것이 Connection Pool 입니다.
- 또한 커넥션을 계속해서 재사용하기 때문에 생성되는 커넥션 수가 일정하게 유지되어집니다.
- 동작과정
  - 애플리케이션을 시작하는 시점에 커넥션 풀은 필요한 만큼 커넥션을 미리 생성하여 보관합니다.
  - 서비스의 특징과 스펙에 따라 생성되는 Connection 의 개수는 다르지만 일반적으로 기본값으로 10개를 생성합니다.
  - 커넥션 풀에 들어있는 Connection는 TCP/IP로 DB와 연결되어 있는 상태이기 때문에 즉시 SQL을 DB에 전달할 수 있습니다.
  - 즉, DB 드라이버를 통해 새로운 Connection을 획득하는 것이 아닌 이미 생성되어 있는 커넥션을 참조하여 사용하게 됩니다.
  - 커넥션 풀에 있는 커넥션을 요청하면 커넥션 풀은 자신이 가지고 있는 커넥션 객체 중 하나를 반환합니다.
  - 커넥션 풀을 통해 커넥션을 사용하고 나면 커넥션을 종료하지 않고 커넥션 풀에 반납합니다.
- 커넥션 풀의 종류 
  - Apache DBCP, C3P0, HikariCP 등이 있습니다.



커넥션 풀의 이점

- 커넥션 풀을 효과적으로 사용하고 관리한다면 , 데이터베이스 연결의 비용을 최소화 하고 애플리케이션의 성능을 향상시킬 수있습니다.

  - 애플리케이션 로딩 시점에 미리 커넥션을 생성하기에 애플리케이션에서 데이터베이스 연결에 필요한 리소스가 줄어들어 성능이 향상됩니다.
  - 데이터베이스 연결시  아이디와 패스워드, 기타 부가 정보를 DB에 전달해야합니다 이 과정을 미리 처리하여 커넥션을 생성하기에 데이터베이스 연결비용이 최소화 됩니다.

  

### 

