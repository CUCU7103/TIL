# HttpserveltRequest/Response와 HttpSession 차이점



### HttpServletRequest /  HttpServletResponse

### 1. **HttpServletRequest** 

- 자바 서블릿 API 입니다.

- **클라이언트가 서버로 보낸 HTTP요청을 나타냅니다.**

- **클라이언트에서 서버로 요청이 발생할 때, 이 객체를 통하여 개발자는 요청에 포함된 데이터를 읽고 분석할 수있습니다.**

  - Http 요청 메시지

    - start line 조회 : HTTP메소드, URL, 쿼리 스트링, 스키마, 프로토콜
    - 헤더 조회
    - 바디 조회 : form 파라미터, message body데이터 직접 조회

    

- GET, POST 등 요청 방식, 요청 파라미터, 헤더(Header) 정보, 쿠키(Cookie) 등 다양한 정보를 조회할 수 있습니다.



**목적**

- **요청 정보 검색**
  - <u>**HttpServletRequest를 사용하면 개발자가 요청 방법, URL, 쿼리 매개변수, 헤더 및 클라이언트의 IP 주소와 같은 들어오는 요청에 대한 필수 세부 정보를 얻을 수 있습니다.**</u>

- **매개변수 처리**
  - 쿼리 문자열, 양식 제출 또는 요청 본문을 통해 클라이언트에서 보낸 요청 매개변수에 액세스하는 방법을 제공합니다.

- **세션 관리**
  - 세션을 생성, 검색 및 무효화하는 방법을 제공하여 세션 관리를 용이하게 합니



**주요 메서드**

1. getParameter(String name) : 요청 파라미터(쿼리 스트링, HTML Form 등)를 가져옵니다

``` java
String username = request.getParameter("username");
```

2. getHeader(String name): 요청 헤더 정보를 가져옵니다

``` java
String userAgent = request.getHeader("User-Agent");
```

3. getCookies(): 클라이언트로부터 전달된 쿠키 정보를 가져옵니다.
4. getSession([boolean create]): 세션을 반환합니다. `create=true` 시 세션이 없으면 새로 생성합니다.
5. getAttribute/setAttribute: Request 범위 내에서 속성을 저장하거나 꺼낼 수 있지만, 일반적으로 Request 범위 스코프를 저장하는 용도로 활용됩니다.

``` java
request.setAttribute("data", "Hello");
String data = (String) request.getAttribute("data");
```







---

### 2. HttpServletResponse

- **개념**

  - **HTTP 응답 메시지를 생성하는데 사용**되어집니다.
  - 응답 코드 지정(2xx, 3xx, 4xx, 5xx 등등) 
  - 바디 생성 가능

- **주요 메서드**

  1. setStatus(int sc): HTTP 상태 코드를 설정합니다. 

     - (예: `response.setStatus(HttpServletResponse.SC_OK)`)

  2. setContentType(String type): 응답 타입을 설정합니다.

     - (예: `response.setContentType("text/html;charset=UTF-8")`)

  3.  getWriter()/getOutputStream() :

     - 응답 바디에 데이터를 작성하기 위한 Writer/OutputStream을 반환합니다.

     ```java
     PrintWriter out = response.getWriter();
     out.println("<html>...</html>");
     ```

  4. addCookie(Cookie cookie): 클라이언트에게 쿠키를 내려보낼 수 있습니다.

  5. sendRedirect(String location): 다른 URL로 리다이렉트합니다.

     

- **사용 예시**

  - 로그인 처리가 성공하면, `response.sendRedirect("/main")` 와 같이 메인 페이지로 리다이렉트할 수 있습니다.
  - 응답에 JSON이나 HTML 문서를 작성하여 클라이언트에게 전송할 수 있습니다.



### 3. HttpSession

- 개념

  - **<u>사용자(브라우저)별 상태(State)를 서버 쪽에서 유지하기 위한 객체</u>**입니다.

  - 서버는 사용자를 구분하기 위해 세션 ID를 발급하고(쿠키 또는 URL Rewriting 등을 통해), 세션 범위 내에서 데이터를 유지합니다.

  - 세션에 저장된 데이터는 **동일한 사용자**가 **다른 요청**(브라우저에서 새로고침하거나 다른 페이지 요청 등)을 할 때도 그대로 사용 가능합니다.



- **주요 메서드**

  1. `setAttribute(String name, Object value)`: 세션에 데이터를 저장합니다.

  - ``` java
    HttpSession session = request.getSession();
    session.setAttribute("loginUser", userObject);
    ```

  2. `getAttribute(String name)`: 세션에 저장된 데이터를 가져옵니다

  - ``` java
    User loginUser = (User) session.getAttribute("loginUser");
    ```

  3. `removeAttribute(String name)`: 세션에 저장된 특정 속성을 제거합니다.

  4. `invalidate()`: 세션 전체를 무효화(제거)합니다. -> 로그아웃 시 자주 사용



| 항목                 | HttpServletRequest / HttpServletResponse                     | HttpSession                                                  |
| -------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **범위(Scope)**      | **Request 범위**(각 요청당 생성되고, 응답 후 소멸)           | **Session 범위**(사용자 브라우저와 서버 간 세션이 유지되는 동안) |
| **저장 기간**        | 한 번의 요청 처리 동안만 유지                                | 브라우저가 종료되거나 로그아웃, 혹은 세션 타임아웃 전까지 유지 |
| **주 사용 목적**     | **요청 정보(파라미터, 헤더, 쿠키 등) 획득<br />응답 제어(상태 코드, 헤더, 바디 등)** | 사용자별 상태 정보 유지<br />로그인 상태, 장바구니 등 상태 데이터 관리 |
| **객체 접근 방법**   | 서블릿의`service()`또는`doGet()`,`doPost()`등의 메서드의 파라미터로 제공 | `request.getSession()`또는`request.getSession(true)`         |
| **데이터 유지 방식** | 요청 -> 응답 간 데이터 전달 (한 번의 요청 주기 안)           | 세션 쿠키(JSESSIONID) 등을 통해 서버가 세션 식별자를 유지    |





