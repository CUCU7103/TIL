# ServletContextListener란?

- **웹 어플리케이션이 시작되거나 종료될 때 호출할 메서드를 정의한 인터페이스입니다.**
  - 웹 컨테이너는 웹 어플리케이션이 시작/종료되는 시점에 특정 클래스의 메서드를 실행할 수 있는 기능을 제공합니다.
  - 이 기능을 통해 실행 시 필요한 웹 어플리케이션의 초기화 작업 또는 종료된 후 사용된 자원을 반환하는 등의 작업을 수행합니다.

- **ServletContextListener 인터페이스에 정의된 두 개의 메서드:**
  - **`contextInitialized(ServletContextEvent sce)`**: 
    - 웹 애플리케이션이 시작될 때 호출됩니다. 
    - 초기화 작업을 수행하는 데 사용됩니다.
  - **`contextDestroyed(ServletContextEvent sce)`**: 
    - 웹 애플리케이션이 종료될 때 호출됩니다. 
    - 정리 작업을 수행하는 데 사용됩니다.



```java
package com.listener;
 
import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;
 
public class ContextListenerTest implements ServletContextListener {
 
    @Override
    public void contextDestroyed(ServletContextEvent sce) {
        //서버가 종료되면 실행되는 리스너 메소드
        System.out.println("서버가 종료됨");
    }
 
    @Override
    public void contextInitialized(ServletContextEvent sce) {
        // 서버가 시작되면 실행되는 리스너 메소드
        System.out.println("서버가 시작됨");
    }
 
}
```



## 주로 사용되는 곳

`ServletContextListener`는 다양한 초기화 및 종료 작업을 수행하는 데 사용됩니다.

대표적인 사용 사례는 다음과 같습니다:

1. **데이터베이스 연결 풀 초기화 및 종료**:
   - 애플리케이션 시작 시 데이터베이스 연결 풀을 생성하고, 종료 시 연결 풀을 종료하여 리소스를 정리합니다.
2. **애플리케이션 범위의 설정 로드**:
   - 설정 파일(예: `properties` 파일)을 로드하여 `ServletContext`에 속성으로 설정하거나, 애플리케이션 전역에서 사용할 설정을 초기화합니다.
3. **캐시 초기화**:
   - 애플리케이션 시작 시 필요한 캐시를 로드하거나 생성하고, 종료 시 캐시를 정리합니다.
4. **스케줄러 시작 및 종료**:
   - 주기적으로 작업을 수행하는 스케줄러(예: Quartz)를 초기화하고, 애플리케이션 종료 시 스케줄러를 중지합니다.
5. **리소스 관리**:
   - 파일 핸들러, 네트워크 연결 등 외부 리소스를 초기화하고 종료 시 정리합니다.
6. **로그 설정**:
   - 애플리케이션 시작 시 로그 설정을 초기화하고, 종료 시 로그 관련 리소스를 정리합니다



#### 예시

- 데이터베이스 연결 풀 초기화

```  java
import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;
import javax.servlet.annotation.WebListener;
import javax.sql.DataSource;
import org.apache.commons.dbcp2.BasicDataSource;

@WebListener
public class DatabaseContextListener implements ServletContextListener {

    private BasicDataSource dataSource;

    @Override
    public void contextInitialized(ServletContextEvent sce) {
        dataSource = new BasicDataSource();
        dataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql://localhost:3306/mydb");
        dataSource.setUsername("user");
        dataSource.setPassword("password");
        dataSource.setInitialSize(10);
        
        // ServletContext에 DataSource 객체를 저장
        sce.getServletContext().setAttribute("DataSource", dataSource);
        System.out.println("데이터베이스 연결 풀이 초기화되었습니다.");
    }

    @Override
    public void contextDestroyed(ServletContextEvent sce) {
        try {
            dataSource.close();
            System.out.println("데이터베이스 연결 풀이 종료되었습니다.");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

```

위의 예제에서는 `BasicDataSource`를 사용하여 데이터베이스 연결 풀을 초기화하고, `ServletContext`에 저장하여 애플리케이션 내의 모든 서블릿과 JSP에서 사용할 수 있도록 합니다. 

애플리케이션이 종료될 때 연결 풀을 닫아 리소스를 정리합니다.



## `web.xml`을 통한 리스너 등록

`@WebListener` 애노테이션 대신 `web.xml` 파일을 사용하여 리스너를 등록할 수도 있습니다:

```
xml코드 복사<web-app>
    <listener>
        <listener-class>com.example.DatabaseContextListener</listener-class>
    </listener>
</web-app>
```







서블릿 리스너의 동작 구조

![image-20241206011606882](C:\typora-image\image-20241206011606882.png)
