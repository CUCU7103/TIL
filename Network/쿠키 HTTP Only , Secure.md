# 쿠키 HTTP Only , Secure

## HTTP Only

- 쿠키는 클라이언트에서 자바스크립트로 조회할 수 있기 때문에 해커들은 자바스크립트로 쿠키를 가로채고자 시도하게 됩니다.
- 가장 대표적인 공격은  XSS (Cross Site Scripting) 입니다.
- 이러한 스크립트를 통한 탈취방법을 막고자 브라우저에서는 쿠키에 접근할 수 없도록 제한하기 위한 옵션을 부여하는데 이것이 **HTTP Only** 입니다

````Set-Cookie: 쿠키명=쿠키값; path=/; HttpOnly````

- 가장 마지막에 **HttpOnly라는 접미사만 추가**함으로써 HTTP Only Cookie가 활성화 되며, 위에서 말한 XSS와 같은 공격이 차단되게 됩니다. 

- **HTTP Only를 설정하면 브라우저에서 해당 쿠키로 접근할 수 없게 되지만, 쿠키에 포함된 정보의 대부분이 브라우저에서 접근할 필요가 없기 때문에 HTTP Only Cookie는 기본적으로 적용하는 것이 좋습니다.**





## Secure

- HTTP Only Cookie를 사용하면 Client에서 Javascript를 통한 쿠키 탈취문제를 예방할 수 있습니다.
- 하지만 Javascript가 아닌 네트워크를 직접 감청하여 쿠키를 가로챌 수도 있습니다.
- **HTTPS 프로토콜을 사용하여 데이터를 암호화하여 서버에 넘겨주게 되면, 해커들이 쿠키를 탈취해도 암호화가 되어있어 정보를 알아낼 수 없습니다.**
- 이를 위해 `Secure` **접미사를 사용**해서 쿠키를 생성하게 되면 브라우저는 **HTTPS 가 아닌 통신에서는 쿠키를 전송하지 않습니다.**



예시

``` Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT; Secure; HttpOnly```
