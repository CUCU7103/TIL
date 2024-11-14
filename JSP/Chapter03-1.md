# chapter 03-1

> [!NOTE]
>
> JSP란?
>
> - JavaServer Pages의 약자를 뜻하며, HTML 코드에 JAVA 코드를 사용하여 동적 웹페이지(Dynamic Web Page)를 생성하는 웹 어플리케이션 도구(라이브러리)이다.
>
> - JSP가 실행되면 자바 "Servlet"으로 변환이 되며, 웹 어플리케이션 서버에서 동작되게 되며, 생성된 데이터들을 웹페이지와 클라이언트를 통해 응답을 한다.
>
> 동적 자원
>
> - 시간이나 특정 조건에 따라 응답 데이터가 달라지는 자원을 의미합니다.



JSP의 구성요소

- 디렉티브 (Directive)

  - JPS 페이지에 대한 설정 정보를 지정할때 사용한다.

- 스크립트 : 

  - 문서의 내용을 동적으로 생성하기 위해 사용되는것이 스크립트 요소이다.
  - 스크립트릿(Scriptlet) : 자바 코드를 실행한다.
  - 표현식(Expression) : 값을 출력한다.
  - 선언부(Declaration) : 자바 메서드를 만든다.

- 표현 언어 (Expression Language)

  - JSP의 스크립트 요소(표현식, 스크립트릿, 선언부) 는 자바 문법을 그대로 사용할 수 있기 때문에 자바 언어 특징을 그대로 사용할 수 있다.

  - 표현 언어는 ${ ... } 로 구성되어 정해진 문법을 따르는 식(값을 생성하는 코드)을 입력한다..

  - ``` jsp
    <%
    
        int a = Integer.parseInt(request.getParameter("a"));
    
        int b = Integer.parseInt(request.getParameter("b"));
    
    %>
    a * b = <%= a * b %>
    표현 언어 사용
    a * b = ${param.a * param.b}
    ```

- 기본 객체 (Implicit Object)

- 정적인 데이터

- 표준 액션 태그 (Action Tag)

  - 액션 태그는 JSP 페이지에서 특별한 기능을 제공한다.

  - 액션 태그의 종류에 따라 서로 다른 속성과 값을 가진다.

   **형태** 

  ```
  <jsp:액션태그이름 속성="값" ... />
   예) <jsp:**include** page="header.jsp" flush="true" />
   지정한 페이지 "header.jsp"의  실행 결과를 현재 위치에 포함시킨다.
  ```

- #### 커스텀 태그(Custom Tag), 

  - JSP를 확장 시켜주는 기능으로 액션태그와 마찬가지로 태그 형태로 제공한다
  - 커스텀 태그는 개발자가 직접 개발해야한다.
  - 일반적으로 JSP 코드에서 중복되는 것을 모듈화하거나, 스크립트 코드를 사용 시 발생하는 소스 코드의 복잡함을 없애기 위해 사용한다.

- **JSTL (JavaServer Page Standard Tag Library)**

  - 커스텀 태그 중에서 자주 사용한 것들을 별도로 표준화 한 태그 라이브러리이다.
  - 조건문, 반복문 같은 처리를 커스텀 태그로 구현할 수 있도록 해준다.



### page 디렉티브

- **JSP 페이지에 대한 정보를 입력**하기 위해 사용된다.
- JSP 페이지가 어떤 문서를 생성하는지, 어떤 자바 클래스를 사용하는지, 세션에 참여하는지, 출력 버퍼의 존재 여부 등 JSP 페이지를 실행하는 데 필요한 정보를 입력할 수 있다



| 속성                           | 설명                                                         | 기본값    |
| ------------------------------ | ------------------------------------------------------------ | --------- |
| contentType                    | JSP가 생성할 문서의 MIME 타입과 캐릭터 인코딩을 지정한다.    | text/html |
| import                         | JSP 페이지에서 사용할 자바 클래스 지정한다.                  |           |
| session                        | JSP 페이지가 세션을 사용할 지 여부 지정한다.false 일 경우 세션을 사용하지 않는다. | true      |
| buffer                         | JSP 페이지의 출력 버퍼 크기를 지정한다.none 일 경우 출력 버퍼를 사용하지 않는다.8kb 일 경우 8kb 크기의 출력 버퍼를 사용한다. | 최소 8kb  |
| autoFlush                      | 출력 버퍼가 다 찼을 경우 자동으로 버퍼 내용을 출력 스트림에 보내고 비울지 여부를 지정한다.true 인 경우 버퍼 내용을 브라우저에 보낸 후 버퍼를 비운다false 인 경우 에러를 발생시킨다. | true      |
| info                           | JSP 페이지에 대한 설명을 입력한다.                           |           |
| errorPage                      | JSP 페이지 실행 도중 에러 발생 시 보여줄 페이지를 지정한다.  |           |
| isErrorPage                    | 현재 페이지가 에러 발생 시 보여주는 페이지 인지 여부를 지정한다. true : 에러 페이지, false : 에러 페이지가 아님 | false     |
| pageEncoding                   | JSP 페이지 소스 코드의 캐릭터 인코딩을 지정한다.             |           |
| isELIgnored                    | true : 표현 언어를 해석하지 않고 문자열로 처리false : 표현 언어를 지원한다. | false     |
| deferredSyntaxAllowedAsLiteral | #{ 문자가 문자열 값으로 사용되는 것을 허용할 지 여부 결정    | false     |
| trimDirectiveWhitespaces       | 출력 결과에서 템플릿 텍스트의 공백 문자를 제거할 지 여부 결정 | false     |



- contentType

  - JSP 페이지가 생성할 문서의 타입을 나타낸다.

  - **; charset="캐릭터셋"** 부분은 생략할 수 있다.

    - 만약 한글을 표현하려면 euc-kr 또는 utf-8 을 사용해야 한다.

       (캐릭터 셋은 대소문자를 구분하지 않는다. utf-8 = UTF-8)

- import
  - page 디렉티브의 import 속성을 사용해서 단순이름을 사용할 수있습니다.
  -  import 속성의 값으로 여러 타입을 지정할 수도 있다. ( , 콤마로 구분한다.)
    - <%@ page import="**java.util.Calendar, java.util.Date**" %>
  - 패키지 이름 뒤에 별표( * ) 를 사용하면 해당 패키지의 모든 타입(클래스)을 단순 이름으로 사용할 수 있다.
    - <%@ page import="**java.util.\***" %>





