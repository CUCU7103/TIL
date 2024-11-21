# HTTP 메시지

- HTTP 메시지는 **서버와 클라이언트 간에 데이터가 교환되는 방식**입니다.
- type
  - request(요청)
    - 클라이언트가 서버로 전달해서 서버의 액션이 일어나게끔 하는 메시지 입니다.
  - response(응답)
    - request에 대한 서버의 답변입니다.

HTTP 요청과 응답의 구조는 서로 닮았으며, 그 구조는 다음과 같습니다.

1. 시작 줄('start-line')에는 실행되어야 할 요청, 또은 요청 수행에 대한 성공 또는 실패가 기록되어 있습니다. 이 줄은 항상 한 줄로 끝납니다.
2. 옵션으로 'HTTP 헤더' 세트가 들어갑니다. 여기에는 요청에 대한 설명, 혹은 메시지 본문에 대한 설명이 들어갑니다.
3. 요청에 대한 모든 메타 정보가 전송되었음을 알리는 빈 줄(' empty line')이 삽입됩니다.
4. 요청과 관련된 내용(HTML 폼 콘텐츠 등)이 옵션으로 들어가거나, 응답과 관련된 문서가 들어갑니다. 본문의 존재 유무 및 크기는 첫 줄과 HTTP 헤더에 명시됩니다.

![image](https://github.com/user-attachments/assets/5463abdb-40a5-479f-acb1-73a4d08d2b95)


HTTP 메시지의 구조는 요청메시지, 응답메시지 모두 위의 구조를 갖습니다.

 empty line에는 반드시 Enter가 한 칸 들어가야 합니다. message body에는 데이터를 넣어도 되고 넣지 않아도 됩니다.



## HTTP request message

![image](https://github.com/user-attachments/assets/a9f11bd7-12b9-409f-b2ae-76518bd35ce2)

- **start-line  (시작줄)**

  ```
  GET /test.html HTTP/1.1
  [HTTP Method] [Request target] [HTTP version]
  ```

  - 구성요소

    - **HTTP 메서드**

      - 영어 동사([`GET`](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/GET), [`PUT`](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/PUT),[`POST`](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/POST)) 혹은 명사([`HEAD`](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/HEAD), [`OPTIONS`](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/OPTIONS))를 사용해 서버가 수행해야 할 동작을 나타냅니다. 
      - 예를 들어, `GET`은 리소스를 클라이언트로 가져다 달라는 것을 뜻하며, `POST`는 데이터가 서버로 들어가야 함을 의미(리소스를 새로 만들거나 수정하기 위해, 또는 클라이언트로 돌려 보낼 임시 문서를 생성하기 위해)합니다.

    - **request target(요청 타겟)**

      - URL, 또는  프로토콜, 포트, 도메인의 절대 경로로 나타낼 수도 있으며 이들은 요청 컨텍스트에 의해 특정지어 집니다.

      - HTTP request가 전송되는 목표 주소입니다.

      - 요청 타겟 포맷은 HTTP 메소드에 따라 달라집니다. 

        - **orgins 형식**

        - 가장 일반적인 형식이고 **'origin 형식'으로 알려진 절대 경로**입니다. 

        - 이 형식은 끝에 `'?'`와 쿼리 문자열이 따라옵니다. `GET`, `POST`, `HEAD`, `OPTIONS` 메서드와 함께 사용합니다.

          ![image](https://github.com/user-attachments/assets/a06c423f-630e-4cfa-9156-ca2f25576e82)


        - **'absolute 형식**'

          - 프록시에 연결하는 경우 대부분 `GET`과 함께 사용됩니다. 
          - `GET http://developer.mozilla.org/ko/docs/Web/HTTP/Messages HTTP/1.1`

        - **'authority 형식'**

          - 도메인 이름 및 옵션 포트(`':'`가 앞에 붙습니다)로 이루어진 URL의 인증 컴포넌트 입니다. 
          - HTTP 터널을 구축하는 경우에만 `CONNECT`와 함께 사용할 수 있습니다. `CONNECT developer.mozilla.org:80 HTTP/1.1`

          

        - **'asterisk 형식'**

          - `OPTIONS`와 함께 별표(`'*'`) 하나로 서버 전체를 나타내는 입니다. `OPTIONS * HTTP/1.1`

          

      - **HTTP version**

        - 응답 메시지에서 써야 할 HTTP 버전을 알려주는 역할

          

      

- Headers (헤더)

  - 다양한 종류의 header가 있는데  아래그림과 같이 몇몇 그룹으로 나눌 수있습니다.


  ![image](https://github.com/user-attachments/assets/47ad207d-5f8d-481a-ade1-b23dd4611d02)


  - **HTTP 전송에 필요한 모든 부가정보가 들어있습니다**.

  - 클라이언트와 서버가 요청 또는 응답으로 부가적인 정보를 전송할 수 있도록 해줍니다.

  - HTTP 헤더는 대소문자를 구분하지 않는 이름과 콜론 '`:`' 다음에 오는 값(줄 바꿈 없이)으로 이루어져있습니다

  - 헤더는 컨텍스트에 따라 그룹핑될 수 있습니다:

    - General header: 

      -  [General 헤더](https://developer.mozilla.org/ko/docs/Glossary/General_header)는 메시지 전체에 적용됩니다.
        

    - **Request header:** 

      - 요청의 내용을 좀 더 구체화 시키고([`Accept-Language`](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Accept-Language)), 컨텍스를 제공하기도 하며([`Referer`](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Referer)), 조건에 따른 제약 사항을 주기도 하면서(`If-None`) 요청 내용을 수정합니다.

        

      - **Host** : 요청하려는 서버 호스트 이름과 포트번호

      - **User-agent** : 클라이언트 프로그램 정보. 이 정보를 통해 서버는 클라이언트 프로그램(브라우저)에 맞는 최적의 데이터를 보내줄 수 있다.

      - **Referer** : 바로 직전에 머물렀던 웹 링크 주소 

      - **Accept** : 클라이언트가 처리 가능한 미디어 타입 종류 나열

      - **If-Modified-Since** : 여기에 쓰여진 시간 이후로 변경된 리소스 취득. 페이지가 수정되었으면 최신 페이지로 교체한다.

      - **Authorization** : 인증 토큰을 서버로 보낼 때 쓰이는 Header

      - **Origin** : 서버로 Post 요청을 보낼 때 요청이 어느 주소에 시작되었는지 나타내는 값. 이 값으로 요청을 보낸 주소와 받는 주소가 다르면 CORS(Cross-Origin Resource Sharing) 에러가 발생한다.

      - **Cookie** : 쿠키 값이 key-value로 표현된다.

    

    -  Representation 헤더
      - 메시지 데이터의 원래 형식과 적용된 인코딩을 설명하는 [`Content-Type`](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Content-Type)과 같은메시지에 본문이 있는 경우에만 존재합니다).



- body (본문)
  - **HTTP Request가 전송하는 데이터를 담고 있는 부분**
  - 전송하는 데이터가 없다면 body 부분은 비어있습니다.
  - 보통 post 요청일 경우, HTML 폼 데이터가 포함되어 있습니다
  - 모든 요청에 본문이 들어가지는 않습니다. `GET`, `HEAD`, `DELETE` , `OPTIONS`처럼 리소스를 가져오는 요청은 보통 본문이 필요가 없습니다.
  -  일부 요청은 업데이트를 하기 위해 서버에 데이터를 전송합니다. 보통 (HTML 폼 데이터를 포함하는) `POST` 요청일 경우에 그렇습니다.



# HTTP Response Body

- HTTP Response Message는 request와 동일하게 공백(blank line)을 제외하고 3가지 부분으로 나누어진다.

![image](https://github.com/user-attachments/assets/281da8a5-79c9-42f2-ae10-83ae808057d0)


- ### status line (상태줄)

  - HTTP 응답의 시작 줄은 '상태 줄(status line)'' 이라고 불리며, 다음과 같은 정보를 가지고 있습니다.
    - 보통 `HTTP/1.1`인 프로토콜 버전입니다.
    - 요청의 성공 여부를 나타내는 '상태 코드'입니다. 일반적인 상태 코드는 [`200`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/200), [`404`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/404) 혹은 [`302`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/302)입니다.
    - 사람이 HTTP 메시지를 이해할 때 도움이 되는 상태 코드에 대한 짧고, 순전히 정보 제공 목적의 '상태 텍스트'
    - 상태 줄은 일반적으로 `HTTP/1.1 404 Not Found.` 같이 표현됩니다.





- Header

  ![image](https://github.com/user-attachments/assets/5bc109b2-0eec-4408-b2e9-28988eefb116)


  - General 헤더는 메시지 전체에 적용됩니다.
  - Response 헤더는 상태 줄에 포함되지 않은 서버에 대한 추가 정보를 제공
  - Representation 헤더



- body

  - Request와 마찬가지로 모든 response가 body가 있지는 않다.
  - 데이터를 전송할 필요가 없을경우 body가 비어있게 된다.

  
