# 서블릿

### 서블릿이란?

![image](https://github.com/user-attachments/assets/5b8e911d-540e-4308-a61d-4d53bde2167c)

- 서블릿은 Dynamic Web page를 만들기 위해 사용되는 자바의 웹 애플리케이션 **프로그래밍 기술**입니다.
  - **웹 요청과 응답의 흐름을 간단한 메서드 호출만으로 체계적으로 다룰 수 있게 해주는 기술**
- **클라이언트의 요청을 처리하고 응답을 생성하는 역할**을 합니다.



### 서블릿 클래스의 구현 

- HttpServlet 클래스를 상속받은 클래스를 작성해야 합니다.
- 처리하고자 하는 HTTP 방식에 알맞은 method를 재정의해서 구현해야 합니다.
  - 예를 들어서 GET 방식의 요청을 처리해야 한다면 doGET() 메서드를 재정의하면 됩니다.
    - doGET()은 기본적으로 HttpServletRequest, HttpServletResponse 객체를 갖습니다.
- 재정의한 메서드는 request를 이용해서 웹 브라우저의 정보를 읽어오던가 response를 사용해서 응답을 전송할 수 있습니다.
- 이때 응답을 전송하려면  response.setContentType()를 사용해서 응답의 컨텐츠 타입을 지정해야 합니다.
- 이후 실제 응답 결과를 웹 브라우저에 전송해야 합니다. 이때 PrintWriter의 print() 메서드에 응답데이터를 담아 전달합니다.
- print() 메서드에 전달한 데이터는  웹 브라우저에 전송되어 화면에 출력되어집니다.

``` java
@WebServlet("/htmlResponse")
public class HtmlResponseServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        // 응답의 콘텐츠 타입을 HTML로 설정
        response.setContentType("text/html;charset=UTF-8");
        
        // PrintWriter를 통해 HTML 내용 작성
        PrintWriter out = response.getWriter();
        try {
            out.println("<!DOCTYPE html>");
            out.println("<html>");
            out.println("<head><title>HTML 응답</title></head>");
            out.println("<body>");
            out.println("<h1>안녕하세요, 서블릿을 통한 HTML 응답입니다!</h1>");
            out.println("</body>");
            out.println("</html>");
        } finally {
            out.close();
        }
    }
}

```

<br>

### 서블릿의 동작과정

![image](https://github.com/user-attachments/assets/51aaf856-4dde-401b-9c45-0d970c36adef)


1. **클라이언트 요청 (HTTP Request)**
   - 클라이언트(예: 브라우저)는 HTTP Request를 생성하여 웹 서버로 요청을 보냅니다.
  
<br>

2. **웹 서버 or WAS**

   - **웹 서버**(예: Apache Tomcat)는 클라이언트 요청을 수신합니다.

   - 요청 URL 를 분석하고, 이 URL과 매핑된 서블릿 객체가 있는지 확인합니다.

   - 해당 요청은 **웹 컨테이너**로 전달됩니다.
  
<br>

3. **웹 컨테이너** (서블릿 컨테이너)

   - 웹 컨테이너는 서블릿의 생명주기(라이프사이클)을 관리하며, 요청을 처리하는 주요 작업을 수행합니다.

   - **`HttpServletRequest` 및 `HttpServletResponse` 객체 생성**

     - 웹 컨테이너는 요청에 대한 정보를 담은 `HttpServletRequest` 객체를 생성합니다.

     - 응답 데이터를 담을 `HttpServletResponse` 객체도 생성합니다.

   - **서블릿 객체 확인 및 생성**

     - 서블릿 객체가 이미 생성되어 있으면, 기존 객체를 사용합니다.

     - 해당 서블릿이 아직 생성되지 않았다면, 서블릿 객체를 생성하고 `init()` 메서드를 호출합니다.
       - 이 과정을 서블릿 로딩이라고 합니다.

<br>

   - **요청 처리**

     - **`service()` 메서드**: HTTP 요청의 타입(GET, POST 등)에 따라 적절한 메서드를 호출합니다.

     - **service() 메소드를 Overriding 하지 않더라도, 부모 클래스인 HttpServlet의 service()가 자동으로 호출된다.**

       - 하위 클래스에 doGet, doPost를 Overriding했을 때 service() 메소드를 Overriding하지 않더라도 HttpServlet의 service() 메소드가 요청에 맞는 메소드를 알아서 호출해 줍니다.

     - 생성된 `HttpServletRequest`와 `HttpServletResponse` 객체를 서블릿 객체의 요청 처리 메서드(`doGet()` 또는 `doPost()`)에 전달합니다.

     - **doGet(), doPost() 메서드 실행**

       - 요청에 따라 서블릿의 `doGet()` 또는 `doPost()` 메서드가 호출됩니다.

       - 해당 메서드 내부에서 요청을 처리하고 필요한 응답을 생성합니다

     - **destroy() 메서드 실행 (종료)**

       - 서블릿이 더 이상 필요하지 않게 되면, 컨테이너는 서블릿을 종료하며 `destroy()` 메서드를 호출합니다.

       - 이 메서드는 리소스 정리 등의 종료 작업을 수행합니다.

     - **HTTP Response 반환**

       - 서블릿이 생성한 응답은 `HttpServletResponse` 객체에 저장되며, 웹 컨테이너를 통해 클라이언트에게 전달됩니다.

       - 클라이언트는 최종적으로 응답 데이터를 수신합니다.

<br>

### 서블릿의 생명주기

- 서블릿의 생명주기는 웹 컨테이너/서블릿 컨테이너가 관리합니다.

![image](https://github.com/user-attachments/assets/68e43af6-d16b-47f6-a6f8-4cb5d7ab4573)


1. init()
   - 서블릿을 초기화 하며 처음 한번만 실행되어집니다.

2. service()
   - 요청/응답(request/response)을 처리하며 요청이 GET인지 POST인지 구분하여 doGet() 또는 doPost() 에 알맞게 처리합니다.

3. destory()
   - 서블릿 종료요청이 있을때 destroy() 메소드가 실행됩니다

