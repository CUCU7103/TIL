# JDBC (Java Database Connectivity)란?



- JDBC(Java Database Connectivity)는 **자바 프로그래밍 언어를 사용해 데이터베이스에 접근할 수 있도록 하는 자바 API이다.**

- 이를 통해서 데이터베이스에 접속하고, SQL을 실행하고, 데이터를 가져오거나 삭제하는 등 데이터를 다룰 수 있게 됩니다.

  [![image](https://private-user-images.githubusercontent.com/107477191/390861267-a5c1ef21-5679-4dbc-b160-6791f0e58a3e.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzMyMjk4NTYsIm5iZiI6MTczMzIyOTU1NiwicGF0aCI6Ii8xMDc0NzcxOTEvMzkwODYxMjY3LWE1YzFlZjIxLTU2NzktNGRiYy1iMTYwLTY3OTFmMGU1OGEzZS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQxMjAzJTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MTIwM1QxMjM5MTZaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT01OTJjN2QwNmM1ODUxYWE5YTliOTM2ZDg2OTExZDJjNzkwZDg1NTZhMGRhMjYwMzIxMmY2ZDFjMWY2OGZjOWJjJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.DtSKJeFlkxCQhbckNOsBPCpbZM873gzBjXKQ392UrLM)](https://private-user-images.githubusercontent.com/107477191/390861267-a5c1ef21-5679-4dbc-b160-6791f0e58a3e.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzMyMjk4NTYsIm5iZiI6MTczMzIyOTU1NiwicGF0aCI6Ii8xMDc0NzcxOTEvMzkwODYxMjY3LWE1YzFlZjIxLTU2NzktNGRiYy1iMTYwLTY3OTFmMGU1OGEzZS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQxMjAzJTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MTIwM1QxMjM5MTZaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT01OTJjN2QwNmM1ODUxYWE5YTliOTM2ZDg2OTExZDJjNzkwZDg1NTZhMGRhMjYwMzIxMmY2ZDFjMWY2OGZjOWJjJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.DtSKJeFlkxCQhbckNOsBPCpbZM873gzBjXKQ392UrLM)

### JDBC에서의 트랜잭션

- 트랜잭션은 DB의 상태를 변경시키기 위해 수행하는 작업 단위입니다.
- 트랜잭션은 작업의 완전성 을 보장해줍니다.
  - 논리적인 작업 셋을 모두 완벽하게 처리하거나 또는 처리하지 못할 경우에는 원 상태로 복구해서 작업의 일부만 적용되는 현상이 발생하지 않게 만들어주는 기능입니다.

### JDBC에서 사용하는 객체

- #### DriverManager

  - JDBC 드라이버를 통해서 커넥션을 만드는 역할

- #### Connection

  - 특정 데이터 원본과 연결된 객체
  - DB의 연결정보를 담고 있는 객체  (ip주소, port번호, 계정명, 비밀번호)

- #### Statement 

  - 연결된 db에 sql문을 전달하고 실행한 후 결과를 받아내는 객체
    - 완성된 SQL문을 실행할 수 있는 객체
  -  Connection객체에 의해 프로그램에 구현되는 일종의 메소드 집합 
  -  **Connection클래스의 createStatement()를 호출하여 객체 생성**
  -  **Statement객체로 SQL문을 String객체로 담아 인자값으로 전달하여 질의 수행**

- #### ResultSet

  -  SELECT문을 사용한 질의 성공시 반환되는 객체 
    - 만일 실행한 SQL문이 SELECT문일 경우 조회된 결과가 result set객체에 들어감 
  - SQL 질의에 해당하는 결과를 담고 있으며 '커서(CURSOR)'를 이용하여 특정 행에 대한 참조를 조작

### JDBC의 동작과정

- 먼저 JDBC API를 사용하기 위해서는 JDBC 드라이버를 먼저 로딩한 후 데이터베이스와 연결해야 한다.
  - JDBC 드라이버는 **JDBC 인터페이스를 구현한 구현체라**고 생각할 수 있으며 **특정 데이터베이스 벤더(Oracle, MySQL, PostgreSQL 등)에 대한 연결과 데이터베이스에 대한 작업을 가능하게 해준다**. 
  - 즉 jdbc 드라이버는 데이터베이스와 직접적인 통신을 관리한다.
- JDBC가 제공하는 **DriverManager가 드라이버들을 관리하고** Connection을 획득하는 기능을 제공한다.
- 이 획득한 Connection을 통해서 데이터베이스에 SQL을 실행하고 결과를 응답받을 수 있다.

![image](https://private-user-images.githubusercontent.com/107477191/390861305-c60e8ef0-8485-489e-bb2a-742025452a5b.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzMyMjk4NTYsIm5iZiI6MTczMzIyOTU1NiwicGF0aCI6Ii8xMDc0NzcxOTEvMzkwODYxMzA1LWM2MGU4ZWYwLTg0ODUtNDg5ZS1iYjJhLTc0MjAyNTQ1MmE1Yi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQxMjAzJTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MTIwM1QxMjM5MTZaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1hOWU3ODFjNTM1MGQ0ZDExNGIwNzhiMzBkYmU3MTE0YjIyNmYzYWNiMzczNTExMjY1OTdkMjIzNGU4ZGEwNjhjJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.5JKOskdsuSrVHbvYG__6_Eb_M8DLKdPWVhESP2nFcnE)]

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



### 진행 순서

![image](https://private-user-images.githubusercontent.com/107477191/390861331-a681ffbc-939f-4290-9d76-e3837c97b479.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzMyMjk4NTYsIm5iZiI6MTczMzIyOTU1NiwicGF0aCI6Ii8xMDc0NzcxOTEvMzkwODYxMzMxLWE2ODFmZmJjLTkzOWYtNDI5MC05ZDc2LWUzODM3Yzk3YjQ3OS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQxMjAzJTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MTIwM1QxMjM5MTZaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0yOGVkY2NkNGY0ODVlYTA3MTAxYjFiZTNmNjFlYjc3YTY5ZjQwMGMwNGVjYmNmZjZjMDczNTgzMDkxYTViY2M0JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.Tj8bYBlLQu83znDFX-XuX4hD1oIlcBwfqcvhCx_OwZI)

### 자원의 반납순서 

- 선언의 역순으로 반납합니다.

  - **ResultSet → Statement → Connection** 순서로 의존성이 존재하기 때문입니다.

    - **`ResultSet`**은 `Statement` 또는 `PreparedStatement` 객체를 통해 생성되며, 이에 의존합니다.

    - **`Statement`**는 `Connection` 객체를 통해 생성되며, 이에 의존합니다.

    - 따라서, 의존 관계의 맨 마지막인 `ResultSet`을 먼저 닫아야 이후 자원을 안전하게 해제할 수 있습니다.

      

- 순서를 지키지 않고 자원을 반납한다면?

  - `Statement`를 먼저 닫고 `ResultSet`을 닫으려고 하면, `ResultSet`은 이미 닫힌 `Statement`에 의존하므로 **`SQLException`**이 발생합니다.
  - 동일한 이유로, `Connection`을 먼저 닫고 `Statement`나 `ResultSet`을 닫으려 하면 오류가 발생하거나 자원이 제대로 닫히지 않을 수 있습니다.

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

[![image](https://private-user-images.githubusercontent.com/107477191/390861379-20df4a2d-cf12-45bd-b8f3-e07c8398076f.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzMyMjk4NTYsIm5iZiI6MTczMzIyOTU1NiwicGF0aCI6Ii8xMDc0NzcxOTEvMzkwODYxMzc5LTIwZGY0YTJkLWNmMTItNDViZC1iOGYzLWUwN2M4Mzk4MDc2Zi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQxMjAzJTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MTIwM1QxMjM5MTZaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0yMWQyNjA5N2M2MTJkOWU0NjkzM2FiMTY4NjViNzdhNzUwNzNkMTc4ODM0NmI0ZjM1YmEzZDFhMjg1NjhjMjNhJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.SNGm3X2xJuystVheSDxV_a0o3FK7wXniK5bQKVXFX94)](https://private-user-images.githubusercontent.com/107477191/390861379-20df4a2d-cf12-45bd-b8f3-e07c8398076f.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzMyMjk4NTYsIm5iZiI6MTczMzIyOTU1NiwicGF0aCI6Ii8xMDc0NzcxOTEvMzkwODYxMzc5LTIwZGY0YTJkLWNmMTItNDViZC1iOGYzLWUwN2M4Mzk4MDc2Zi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQxMjAzJTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MTIwM1QxMjM5MTZaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0yMWQyNjA5N2M2MTJkOWU0NjkzM2FiMTY4NjViNzdhNzUwNzNkMTc4ODM0NmI0ZjM1YmEzZDFhMjg1NjhjMjNhJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.SNGm3X2xJuystVheSDxV_a0o3FK7wXniK5bQKVXFX94)

- 매번 사용할때마다 커넥션을 생성하는 것은 네트워크와 서버의 자원을 매번 사용해야 합니다.

  - 이는 리소스의 낭비이자 비효율적입니다.

  - 이러한 문제점을 해결하기 위해 커넥션 풀을 사용합니다.

  - 사용하는 네트워크의 자원

    - TCP/IP 통신에서 3-way-handshake, 4-way-handshake가 매번 발생함.

    

## 커넥션 풀이란? (Connection pool)

![image](https://private-user-images.githubusercontent.com/107477191/390861403-b49f989b-d7b5-42ed-810a-7cbc16646019.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzMyMjk4NTYsIm5iZiI6MTczMzIyOTU1NiwicGF0aCI6Ii8xMDc0NzcxOTEvMzkwODYxNDAzLWI0OWY5ODliLWQ3YjUtNDJlZC04MTBhLTdjYmMxNjY0NjAxOS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQxMjAzJTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MTIwM1QxMjM5MTZaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1lNDdjMDBlYzA1NTgzNTc1NGJkOWZlOTgwZGJjMmVkNGZiMTlkODNiMjk5MTA1YzhlZGJhNmRhYmMyYmJiMmQ3JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.Wdo_FngLmDekEbk5QDs5IgdopsiC-EuNNcU-B7gw3nU)

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

### 커넥션 풀의 이점

- 커넥션 풀을 효과적으로 사용하고 관리한다면 , 데이터베이스 연결의 비용을 최소화 하고 애플리케이션의 성능을 향상시킬 수있습니다.
  - 애플리케이션 로딩 시점에 미리 커넥션을 생성하기에 애플리케이션에서 데이터베이스 연결에 필요한 리소스가 줄어들어 성능이 향상됩니다.
  - 데이터베이스 연결시 아이디와 패스워드, 기타 부가 정보를 DB에 전달해야합니다 이 과정을 미리 처리하여 커넥션을 생성하기에 데이터베이스 연결비용이 최소화 됩니다.
