# [JDBC] Statement vs. PreparedStatement

### Statement와 PreparedStatedment는  jdbc에서 sql문을 실행하는 객체입니다.

> [!NOTE]
>
> JDBC에서 SQL문은 3가지 단계로 실행되어집니다.
>
> - Parsing (쿼리 문장 분석)
>   - SQL 문장을 구문 분석하여 SQL의 **문법적 유효성**을 확인합니다.
>   - 이 과정에서 **토큰화(Tokenization)** 및 SQL 키워드와 식별자(테이블 이름, 컬럼 이름 등)를 처리합니다.
>
> - Compilation (컴파일)
>   - SQL 문장을 실행 가능한 형태로 **컴파일**합니다.
>   - 컴파일 과정에서 **쿼리 최적화(Query Optimization)** 를 수행하여 최적의 실행 계획을 생성합니다.
>
> - Excution (실행)
>   - 생성된 실행 계획에 따라 SQL 문을 실행하고 결과를 반환합니다.
>   - 쿼리가 데이터베이스에서 읽기 또는 쓰기 작업을 수행합니다.



## 차이점

### Statement 

- 정적 SQL 문을 실행하고 생성된 결과를 반환하는 데 사용되는 객체입니다.

- 매번 새로운 SQL문을 만들기 때문에 3가지 단계가 반복되어집니다.
- 매번 SQL 문장을 데이터베이스가 새로 분석하고 컴파일하기 때문에 동일한 쿼리를 반복 실행하면 **불필요한 오버헤드**가 발생합니다.
  - 이러한 이유로 주로 DDL에 사용되어집니다.
- SQL Injection 공격에 취약합니다.
  - 클라이언트로부터 받은 값(문자열)을 그대로 쿼리문으로 사용하기에 외부에서 조작하기 쉽습니다.
  - 악의적으로 해커가 where 1=1 연산을 통해 모든 데이터를 조회할 수도 있다. 

``` java
Statement stmt = connection.createStatement();
String sql = "SELECT * FROM users WHERE id = 1";
ResultSet rs = stmt.executeQuery(sql);
```



### PreparedStatement

- SQL 템플릿을 사전에 전달하고 컴파일된 실행 계획을 **캐싱**하여 재사용합니다.
- 반복적인 SQL문 , 동적 SQL문 사용에 효과적입니다.

- **최초 실행**
  - **Parsing**: SQL 템플릿(`SELECT * FROM users WHERE id = ?`)을 전달받아 분석합니다.
  - **Compilation**: **<u>매개변수를 처리할 실행 계획을 생성하고 캐싱합니다.</u>**
  - **Excution **: **바인딩된 매개변수를 실행 계획에 적용**하여 실행합니다.
- **반복 실행(최초 이후) 실행**
  - **분석 및 컴파일 단계는 생략되고, 기존의 캐싱된 Excution 계획을 재사용하여 성능을 최적화**합니다.
  - 매개변수만 다시 바인딩하고 실행합니다.
- **파라미터 바인딩**
  - 플레이스홀더(?)에 파라미터 바인딩이 가능합니다.
  - 객체를 바인딩 할 수있기 때문에 이미지, 파일 등 바이너리 데이터를 다루는 것도 가능합니다.
- **SQL Injection 공격을 방지할 수있습니다.**
  - 문자열을 그대로 쿼리문으로 사용하지 않고 파라미터 바인딩을 통해 SQL Injection을 방지할 수 있습니다.

``` java
String sql = "SELECT * FROM users WHERE id = ?";
PreparedStatement pstmt = connection.prepareStatement(sql);
pstmt.setInt(1, 1); // 실행 1
ResultSet rs1 = pstmt.executeQuery();

pstmt.setInt(1, 2); // 실행 2
ResultSet rs2 = pstmt.executeQuery();
```







