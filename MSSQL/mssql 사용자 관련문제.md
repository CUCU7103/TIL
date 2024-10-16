# MSSQL 사용자 관련문제

> [!NOTE]
>
> MSSQL 관련 정보
>
> ### 1. 스키마란 ?
>
> - **데이터베이스 개체에 대한 네임스페이스**로 **개체가 갖는 고유한 이름을 결정**지어주는 것을 뜻한다
>
> ### 2. 데이터베이스 사용자 검색
>
> - 특정 데이터베이스 내의 사용자 목록을 조회하려면 `sys.database_principals` 시스템 뷰를 사용합니다.
>
> ``` 
> USE yourdatabase; -- 실제 데이터베이스 이름으로 변경하세요
> 
> SELECT
>     name AS UserName,
>     type_desc AS UserType,
>     default_schema_name AS DefaultSchema
> FROM sys.database_principals
> WHERE type IN ('S', 'U') -- SQL 사용자와 Windows 사용자를 포함
>   AND name NOT LIKE '##%'; -- 시스템 사용자를 제외
> ```
>
> ### 3. 서버 로그인 검색
>
> - 서버 수준에서 존재하는 로그인 계정 목록을 조회하려면 `sys.server_principals` 시스템 뷰를 사용합니다.
>
> - ```
>     SELECT
>       name AS LoginName,
>       type_desc AS LoginType,
>       is_disabled AS IsDisabled
>   FROM sys.server_principals
>   WHERE type IN ('S', 'U') -- SQL 로그인과 Windows 로그인을 포함
>     AND name NOT LIKE '##%'; -- 시스템 로그인을 제외
>   ```
>
> ### 4. 데이터베이스 사용자와 서버 로그인 간의 관계
>
> - 로그인(Login): SQL Server 인스턴스 수준에서 인증을 담당하며, 서버에 연결할 수 있는 권한을 제공합니다.
> - 사용자(User): 특정 데이터베이스에 대한 접근과 권한을 관리하며, 로그인과 매핑됩니다.



## 문제

mssql 마이그레이션 작업 후 기본 스키마 말고 새로운 스키마에 db 가 생성되었습니다.

![image](https://github.com/user-attachments/assets/5f681803-d645-4fa8-84a3-9e3bf4a892ac)

mssql을 docker-compose로 생성할때 설정한 계정으로 스프링 부트 db 설정을 진행했고 

정상적으로 연동되는 것을 확인하였습니다.

myriadb와 mssql이 문법적 차이가 있기에 쿼리를 수정해야되는 것은 이미 알고 있었으나 기본 스키마를 설정하는데서 문제가 발생했습니다.

- 발생한 문제
  - db 사용자의 기본 스키마가 dbo로 지정되어있어 데이터를  못찾는 문제 발생
  - jpa는 default_schema 옵션으로 해결
  - 하지만 mybatis에서 일일이 변경해야되는 필요성이 생김

- 과정

  - 신규 사용자를 등록하였음. 

  - 신규 사용자를 등록하고 스키마에 대한 권한 부여 진행하는데 오류가 발생함.

  - ``` 
    SQL Error [15151] [S0001]: Cannot find the user 'newone_mssql', because it does not exist or you do not have permission.
    ```

  - 신규  사용자를 등록할때 서버 수준에서만 등록되어져 있던것이 문제

    - 서버 로그인은 서버 수준에서만 정의된 것이고, 각 데이터베이스에서 이 로그인에 대한 **사용자(user)**를 만들어 권한을 부여해야 합니다.

      

  - #### **사용자 생성 (사용자가 없는 경우)**

    로그인 계정을 데이터베이스 사용자로 등록하고, 기본 스키마 및 권한을 설정해야 합니다.

    ```
    sql코드 복사USE yourdatabase; -- 실제 데이터베이스 이름으로 변경하세요
    
    CREATE USER newone_mssql FOR LOGIN newone_mssql WITH DEFAULT_SCHEMA = dbo; -- 기본 스키마
    ```



