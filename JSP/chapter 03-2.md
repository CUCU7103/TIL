# chapter 03-2



### 스크립트 요소

- **스크립트릿** (Scriptlet)
- **표현식** (Expression)
- **선언부** (Declaration)

스크립트 요소는 JSP 프로그래밍에서 로직을 수행하는데 필요하다.

스크립트 코드를 사용해서 프로그램이 수행해야 하는 기능을 구현할 수 있다.



## 1. 스크립트릿 <% ~ %>

JSP 페이지에서 자바 코드를 실행할 때 사용하는 코드 블록이다.

 **형태**

```
 <%

  자바코드1;

  자바코드2;

  . . .

%>
```



## 2. 표현식

어떤 값을 출력 결과에 포함시키고자 할 때 사용된다.

``` jsp
<%= 값 %>
```

표현식은  ''<%='' 로부터 시작해서 ''%>'' 로 끝나고 이 둘 사이에는 출력할 값이 위치합니다.



## 3. 선언부 <%! ~ %>

JSP 페이지의 스크립트릿이나 표현식에서 사용할 수 있는 **메소드**를 작성 시 선언부를 사용한다.

**형태**

 ```jsp
<%!

  public 리턴타입 메소드이름(매개변수선언1, ... ) {

  자바코드1;

   . . .   return 값;

  }

%>


 ```



> [!NOTE]
>
> - #### URI= 식별자, URL=식별자+위치
>
> - URI는 인터넷상의 리소스 **“자원 자체”**를 식별하는 고유한 문자열 입니다.
>
> - URL은 Uniform Resource Locator, **네트워크상에서 통합 자원(리소스)의** **“위치”** 를 나타내기 위한 규약
>
> - **특정** **웹 페이지**의 주소에 접속하기 위해서는 **웹 사이트**의 주소뿐만 아니라 프로토콜(https, http, sftp, smp 등)을 함께 알아야 접속이 가능한데, 이들을 모두 나타내는 것이 URL입니다.

![image-20241117205902635](https://raw.githubusercontent.com/CUCU7103/typora_images/main/image/image-20241117205902635.png?token=AZT7RR5JCXKVE4TXQXNVFCLHHHNMI)

## **URI와 URL의 차이점** 

#### **|** URI= 식별자, URL=식별자+위치

- ##### **elancer.co.kr**은 URI입니다. 리소스의 이름만 나타내기 때문입니다.

- ##### 반면, **https://elancer.co.kr**은 URL입니다. 이름과 더불어, 어떻게 도달할 수 있는지 위치까지 함께 나타내기 때문입니다. (프로토콜 ‘https’ 포함)

## request 기본 객체

- request 기본객체는 jsp 페이지에서 가장 많이 사용되는 기본 객체로서 웹 브라우저의 요청과 관련이 있다.

- 웹 브라우저에 웹 사이트의 주소를 입력하면 웹 브라우저는 해당 웹 서버에 연결한 후 요청 정보를 전송하는데  이 요청 정보를 제공하는 것이 바로 request 기본 객체이다.

  - JSP 페이지에서 가장 많이 사용되는 기본 내장 객체

  - 웹 브라우저에서 서버의 JSP 페이지로 전달하는 정보를 저장

  - 폼 페이지로부터 입력된 데이터를 전달하는 요청 파라미터 값을 JSP 페이지로 가져옴

웹 브라우저/서버 관련 메서드

![image-20241117203947079](https://raw.githubusercontent.com/CUCU7103/typora_images/main/image/image-20241117203947079.png?token=AZT7RR6WR5NVNTNHZIDRQMLHHHLEC)

#### **요청 파라미터 관련 메소드**

![image-20241117204017400](https://raw.githubusercontent.com/CUCU7103/typora_images/main/image/image-20241117204017400.png?token=AZT7RR4VDU72DP2UFF57A6LHHHLF6)

GET 방식 전송과 POST 방식 전송

- 웹 브라우저는 GET과 POST 방식의 두 가지 방식 중 한 가지를 이용해서 파라미터를 전송합니다.

- GET 방식
  - 요청 URL에 파라미터를 붙여서 전송합니다.
  - URL의 경로 뒤에 쿼리 문자열 (Query string)을 붙여 전송합니다.
  - 전송방법
    - <a> 태그의 링크에 query string을 추가
    - form 태그의 메서드 속성을 get으로 하여 전송
    - 웹 브라우저의 주소에 직접 query string을 추가하는 url 입력

- POST 방식

  - 전송 데이터가 노출되지 않습니다.

    - http 패킷의 body에 데이터를 담아 보내기 때문입니다.
    - **application/x-www-form-urlencoded**
      -  BODY에 key 와 value 쌍으로 데이터를 넣는다. 똑같이 구분자 &를 사용합니다.
    - **text/plain, **
      - BODY에 단순 txt를 넣습니다.
    - **multipart/form-data**
      - 파일전송을 할때 많이 쓰는데 BODY의 데이터를 바이너리 데이터로 넣는다는걸 알려줍니다.

     

## response 기본 객체

- 사용자의 요청을 처리한 결과를 서버에서 웹 브라우저로 전달하는 정보를 저장하고 서버는 응답 헤더와 요청 처리 결과 데이터를 웹 브라우저로 보냅니다.

- 웹 브라우저에 보내는 응답 정보를 담습니다.

- 주요기능

  - 헤더 정보 입력
  - 리다이렉트

- 주요 메서드

  - response.addCookie(Cookie cookie) : 쿠키 정보를 추가한다.

  - response.sendRedirect(String location) : 현재 페이지에서 다른 페이지("지정한 url")로 이동한다.

  - response.sendStatus(int status-code) : 상태 정보를 클라이언트로 전송한다.

  - response.sendError(int error-code) : 에러 정보를 클라이언트로 전송한다.

    

캐시

- 동일한 데이터를 중복해서 로딩하지 않도록 할때 사용합니다.
- 웹 브라우저는 첫 번째 요청시 응답 결과를 로컬 pc의 임시 보관소인 캐시에 저장합니다.
- 이후 동일한 URL에 대한 요청이 있으면 WAS에 접근하지 않고  캐시베 보관된 데이터를 사용합니다.



리다이렉트 

- 웹 서버가 웹 브라우저에게 다른 페이지로 이동하라고 응답하는 기능
  - 특정 페이지를 실행한 후 지정한 페이지로 이동하길 원할 때 리다이렉트 기능을 사용한다.
