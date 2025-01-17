# Chapter 02

- URL 
  - URL은  Uniform-Resource Locator의 약자로 주소와 같은 역할을 합니다.
  - 프로토콜 , 호스트명, 포트번호, 패스 , 쿼리 파라미터로 구성되어져있습니다.
  
  ![image](https://github.com/user-attachments/assets/29e956b0-a15c-4462-843f-b1fd068c360e)


> [!NOTE]
>
> - 프로토콜 
>
>   - 서로 다른 컴퓨터 간에 통신을 하기 위한 규약
>
>   - 웹 페이지의 주소를 표현할 때는 **http**(Hyper Text Transfer Protocol)를 사용함.
>
>   - 웹 브라우저가 서버와 내용을 주고 받을 때 사용하는 규칙입니다.
>
> 
>
> - 호스트명: 
>
>   - 웹 페이지를 요청할 서버의 이름을 지정합니다.
>
>   - 서버 이름은 "lxxyeon.tistory.com/"와 같은 도메인 이름이나 "127.0.0.1"과 같은 IP주소로 입력할 수 있음
>
>     이때, 도메인은 상위도메인, 도메인이름, 호스트명으로 구성할 수있습니다.
>
>   ![image](https://github.com/user-attachments/assets/342a2a74-17c0-4374-8701-57bb8618d93d)
>
>
> - 포트번호
>
>   - 웹서버에서 자원을 접근하기 위해 사용하는 "관문(gate)"
>   - 한 컴퓨터에서 실제로는 IP주소로 하나의 연결이 되어 있지만, 여러 개의 커넥션을 가질 수 있게 하는 논리적인 단위입니다.
>
> - 파일경로
>
>   - 웹서버에서 자원에 대한 경로.
>   - 실제 물리적 경로가 아니고 웹서버에서 추상화한 경로
>
> - 쿼리
>
>   - 추가로 서버에 보내는 **파라미터**.
>
>     같은 경로라 하더라도 **입력한 값**에 따라 다른 결과를 보여줘야할 때 쿼리 문자열을 사용.
>
>     주로 검색 결과를 보여주는 경우에 검색어를 전달하는 용도로 많이 쓰임
>
> - 부분 식별자
>
>   - URL이 지정하는 **자원의 세부 부분**을 지정할때 쓰임


<br>

### DNS (Domain Name System)

- 인터넷에서 도메인 이름을 컴퓨터가 이해할 수 있는 IP 주소로 변환해 주는 **전화번호부**와 같은 시스템

- ####  DNS의 기본 개념

  - **도메인 이름**: 사용자가 웹 브라우저에 입력하는 읽기 쉬운 이름(예: `google.com`).
  - **IP 주소**: 인터넷에서 장치를 식별하는 고유한 숫자 주소(예: `172.217.16.196`).
  - **DNS**: 도메인 이름을 해당 IP 주소로 변환하는 시스템
    - 사용자가 'naver.com' 또는 'google.com'과 같은 도메인 이름을 웹 브라우저에 입력하는 경우 DNS는 해당 사이트의 **올바른 IP 주소**를 찾는 역할을 진행합니다.

- #### 동작과정

  -  사용자가 브라우저에 `www.naver.com`을 입력하면 다음과 같은 과정이 일어납니다.

  **브라우저**는 로컬 캐시를 확인하고, 없으면 **운영체제**의 DNS 캐시를 확인합니다.

  **재귀적 DNS 서버**에 요청을 보냅니다.

  **루트 DNS 서버**는 `.com` TLD 서버의 주소를 반환합니다.

  **TLD DNS 서버**는 `naver.com`의 권한 있는 DNS 서버 주소를 반환합니다.

  **권한 있는 DNS 서버**는 `www.naver.com`의 IP 주소를 반환합니다.

  **브라우저**는 받은 IP 주소로 웹 서버에 연결하여 웹 페이지를 로드합니다

 

### 클라이언트와 서버

일반적으로 네트워크 프로그램에서 요청하는 쪽을 클라이언트라고 부르고 요청을 받아 알맞은 기능이나 데이터를 제공하는 쪽을 서버라고 칭합니다.



### HTML 

- 웹 페이지를 작성할 때 사용하는 문서
- 마크업 언어
- 웹 서버는 URL에 해당하는 HTML 문서를 전송하고 웹 브라우저는 정해진 규칙에 따라서 문서를 분석하여 알맞은 화면을 생성하는데 이를 렌더링이라고함.



### HTTP

- 웹 사이트를 구성하는 html 파일을 전송하기 위해 사용되어지는 프로토콜입니다.
- HTTP 전송은 HTTP request와 HTTP response로 이루어집니다.

![image](https://github.com/user-attachments/assets/4783e2b1-5005-49c7-b427-2f4b626a6971)


![image](https://github.com/user-attachments/assets/c7d676bd-f862-4d01-bf89-2807719d78c3)


### Request Message의 구조

- **Start Line**

  - HTTP Request Message의 시작 라인

  - 3가지 부분으로 구성

    - HTTP method
    - Request target
    - HTTP version

    

- **Headers**

  - request에 대한 추가 정보(addtional information)를 담고 있는 부분입니다.

    - 브라우저의 종류나 , 언어등의 정보를 보낸다.

    

- **Body**

  - 전송하는 데이터를 담고 있는 부분입니다.



Response Message의 구조

- **Status Line**
  -  HTTP Response의 상태를 간략하게 나타내주는 부분
- **Headers**
  - Response에 대한 정보를 전송
    - 응답의 몸체가 어떤 데이터인지, 길이는 어떻게 되는지
- **Body**
  - 전송하는 데이터를 담고 있는 부분입니다.
 

### HTTP 통신

HTTP 통신은 클라이언트(Front-End)와 서버(Back-End)로 나뉘어진 구조로 되어있다.

클라이언트가 요청(Request)하면 서버가 응답(Response) 하는 것이다.

예를들어 클라이언트가 HTTP 메세지를 만들어 보내고 , 서버에서 요청에 대한 응답이 올 때까지 기다린다. 그리고 서버는 요청에 대한 결과를 만들어서 응답한다.



### 정적자원과 동적자원

- **정적자원 (= 정적 페이지)** 

  - 웹 브라우저가 늘 같은 응답 데이터를 받아서 화면을 출력하는 URL에 해당하는 자원 

  - CSS , HTML 등을 파일을 정적 자원으로 제공 

- **동적 자원 (= 동적 페이지)** 

  - 파일을 바꾸지 않아도 조건에 따라 다른 응답 데이터를 전송 

  - 시간이나 특정 조건에 따라 응답 데이터가 달라지는 자원 



## JSP

- 동적 페이지를 생성하는데 필요한 자바의 표준 기술로써 html 응답을 생성하는데 필요한 기능을 제공합니다.
  - xml, json , 바이너리 파일들도 응답으로 생성할 수 있지만 주로 html 응답을 생성
- jsp 통해 만든 프로그램을 실행하려면 WAS가 필요합니다.
- JavaServer Pages의 약자를 뜻하며, HTML 코드에 JAVA 코드를 사용하여 동적 웹페이지(Dynamic Web Page)를 생성하는 웹 어플리케이션 도구(라이브러리)이다.
- JSP가 실행되면 자바 "Servlet"으로 변환이 되며, 웹 어플리케이션 서버에서 동작되게 되며, 생성된 데이터들을 웹페이지와 클라이언트를 통해 응답을 한다.

> [!NOTE]
>
> ### 웹 서버(Web Server)
>
> - 웹 서버는 **클라이언트의 요청에 따라 HTML, CSS, JS, 이미지 파일과 같은 정적 파일을 응답**하여 제공하는 소프트웨어를 말한다.
> - HTTP 프로토콜을 사용하여 동작합니다.
> - 정적 콘텐츠를 처리하고, 동적인 요청은 처리할 수 없습니다.
>
> ### WAS(Web Application Server)
>
> - 웹 서버가 할 수있는 기능의 대부분을 수행가능합니다.
> - HTTP 프로토콜을 사용하여 동작합니다.
> - **클라이언트 요청에 대해 동적인 처리를 담당하는 영역**입니다.
> - 웹 서버와 달리 애플리케이션 로직을 실행할 수 있도록 구성되어 있습니다. 
>   - 예를 들어 회원가입이나 로그인 등의 로직을 처리하는 영역이 WAS 입니다. 
>   - 또한 데이터베이스 연동, 트랜잭션 관리, 보안, 로깅 등의 기능도 제공합니다.





