### 웹 서버(Web Server)

- 웹 서버는 **클라이언트의 요청에 따라 HTML, CSS, JS, 이미지 파일과 같은 정적 파일을 응답**하여 제공하는 소프트웨어를 말한다.
- HTTP 프로토콜을 사용하여 동작합니다.
- 정적 콘텐츠를 처리하고, 동적인 요청에는 처리할 수 없다.

### WAS(Web Application Server)

- 웹 서버가 할 수있는 기능의 대부분을 수행가능합니다.
- HTTP 프로토콜을 사용하여 동작합니다.
- **클라이언트 요청에 대해 동적인 처리를 담당하는 영역**입니다.
- 웹 서버와 달리 애플리케이션 로직을 실행할 수 있도록 구성되어 있습니다. 예를 들어 회원가입이나 로그인 등의 로직을 처리하는 영역이 WAS 입니다. 또한 데이터베이스 연동, 트랜잭션 관리, 보안, 로깅 등의 기능도 제공합니다.

## 차이점

웹 서버는 정적인 데이터를 처리하는 서버입니다. 

이미지나 단순 html 같은 정적인 리소스들을 전달하며, WAS만을 이용할 때보다 빠르고 안정적으로 기능을 수행합니다. 

반면 WAS는 동적인 데이터를 위주로 처리하는 서버입니다.

 DB와 연결되어 사용자와 데이터를 주고받고, 조작이 필요한 경우 WAS를 활용합니다

일반적인 웹 서비스는 

클라이언트  → 웹 서버 → WAS → DB 순서로 요청이 되며 응답은 역순입니다.

![](https://postfiles.pstatic.net/MjAyMzAyMjZfMTA0/MDAxNjc3MzgwMDgwNTMy.UrHitR6JiastCmVc4xVzi14AnyPQoHYNKlbbsYwbfkUg.Pqj_hBd_e7otK-B7u3QYo98PyLA4xIrS-zoHzrhZfckg.PNG.gi_balja/fffff.png?type=w966)

![](https://yozm.wishket.com/media/news/1780/image009.png)

그런데 WAS는 웹 서버의 대부분의 역할을 수행 할 수 있습니다.

그렇다면,  WAS만 사용하여 정적, 동적 역할을 수행하면 되지 않나? 라는 생각이 듭니다.

하지만 WAS가 웹 서버의 역할을 수행할 수 있음에도 웹 서버와 함께 사용하는 이유는 

1. 정적 컨텐츠의 처리분리 (역할의 분리)
    - WAS는 동적 컨텐츠 처리에 집중해야하는데  정적 요청까지 처리하게 되어지면 WAS의 부담이 커지게 되고 성능 저하가 발생할 수 있습니다.
   <br>
2. 웹 서버는 보안 기능을 제공합니다.
    - 웹서버는 SSL/TLS 암호화 통신, HTTP/2 지원, 요청 필터링과 같은 보안 기능을 제공하여 웹 페이지에 대한 접근을 제어할 수 있습니다.
   <br>
3. 로드 벨런싱
    - 웹 서버는 트래픽을 WAS에 분산하여 각 서버가 과부하 상태에 빠지지 않도록 하며 서버간의 자원 활용을 최적화 합니다
    <br>
4. 독립성 ( 구조적 이점)
    - **웹 서버와 WAS**를 **서로 다른 역할을 가진 별도의 컴포넌트**로 운영하여 **각각의 기능을 독립적으로 관리하여**  한쪽 서버에 문제가 생기더라도 다른 서버가 영향을 받지 않고 기능을 유지하도록합니다.