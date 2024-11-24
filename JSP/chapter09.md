# Chapter09

### 쿠키란?

- **서버가 사용자의 웹 브라우저에 전송하는 작은 데이터 조각**입니다.

  - 즉 웹 브라우저가 보관하는 데이터입니다.

- 브라우저는 그 데이터 조각들을 저장해 놓았다가, 동일한 서버에 재 요청 시 저장된 데이터를 함께 전송합니다.

- **사용자를 식별하기 위해 주로 사용**되어집니다.

  - 사용자 행동을 기록하고 분석하는 용도로도 사용됨.

- HTTP 프로토콜의 특징은 상태정보를 유지하지 않는 Statless입니다.

  - 연결을 종료하는 순간 클라이언트와 서버와의 통신은 종료되어지며 상태정보를 유지하지 않습니다.
  - 이는 현재 접속한 사용자가 이전에 접속한 사용자와 같은 사용자인지 서버가 판단할 수가 없습니다.
  - 따라서 http 프로토콜에서 상태를 유지하기 위해서 서버는 요청에 대한 응답을 할때 쿠키라는 것을 같이 보냅니다.

- 서버로 쿠키를 전달받으면 웹 브라우저는 쿠키를 저장해 두었다가 차후 요청이 발생할때마다 쿠키를 같이 보냅니다.

  

---

### 쿠키의 동작방식 (로그인 예시)

![image](https://github.com/user-attachments/assets/bf529618-78b0-4c26-bbaf-a368ed45376b)


- 로그인(/login) 화면에서 클라이언트가 username 과 password를 입력하고 서버에 제출(요청) 한다.
- 서버는 DB를 조회하여 해당 유저 정보 데이터를 가져와 검증하고 세션 메모리에 저장한다. 그리고 이 세션 데이터를 식별하는 세션 아이디(SessionID)를 쿠키로 만들어 반환한다. (Set-Cookie) 
- 클라이언트는 응답 받은 쿠키를 브라우저에 저장해 놓는다. 
  - 그리고 나중에 대시보드 창(/dashboard)에 진입하기 위해서 서버에게 요청을 보낼때 쿠키를 같이 실어 보낸다.
- 서버는 클라이언트로부터 온 쿠키에 들은 세션 아이디를 파싱하여 올바른지 검증하고, 로그인한 회원이면 그에 걸맞는 대시보드 페이지를 응답한다.

---

### 전체적인 흐름을 보자면.

- 웹 서버가 생성한 쿠키가 웹 브라우저로 전송되어집니다.
  - 이때 쿠키는 http message의 header에 담겨 전송되어집니다.
- 웹 브라우저는 수신한 쿠키를 생성시 정해진 기간 또는 웹 사이트의 사용자 세션 기간동안 보관합니다.
- 웹 브라우저는 저장한 쿠키를 요청이 있을 때마다 웹 서버에 전송합니다.
  - 웹 브라우저에 쿠키가 저장되면 웹 브라우저는 쿠키가 삭제되기 전 까지 웹서버에 쿠키를 전송한다.

---

### 구성요소

쿠키는 다음 구성요소로 이루어져 있다.

- **쿠키의 이름 (name)**

- **쿠키의 값 (value)**

  - 쿠키의 값을 변경하려면?
    - 쿠키의 값을 변경하려면 같은 이름의 쿠키를 생성해서 응답 데이터로 보내야 합니다.
    - 쿠키의 값을 변경한다는 것은 기존에 존재하는 쿠키의 값을 변경한다는 것이기에 먼저 대상 쿠키가 존재하는지 확인해야 합니다.

- **쿠키의 유효시간 (Expires)**

  - 쿠키는 유효시간을 갖고있으며 쿠키의 유효시간을 지정하지 않으면 웹 브라우저를 종료할때  쿠키를 같이 제거합니다.
  - 이때 웹 브라우저 종료 후 다시 웹 브라우저를 실행하면 삭제한 쿠키는 서버에 전송되지 않습니다.
  - 쿠키의 유효시간을 정해 놓으면 그 유효시간동안 쿠키가 존재하며 웹 브라우저를 종료해도 유효시간이 지나지 않았으면 쿠키를 삭제하지 않습니다.

- **쿠키를 전송할 도메인 이름 (Domain)**

  - 쿠키의 도메인

    - 기본적으로 쿠키는 그 쿠키를 생성한 서버에만 전송되어집니다.

      - 예시로 http://example.host.com에서 생성된 쿠키는 다른 사이트로 전송되어지지 않는다는 뜻

      - 하지만 같은 도메인을 사용하는 모든 서버에 쿠키를 보내야 할 경우가 발생할 때 별도의 설정이 가능하다.

- **쿠키를 전송할 경로 (Path)**

- **보안 연결 여부 (Secure)**

- **HttpOnly 여부 (HttpOnly)**

---
## **HTTP Cookie  헤더**

### 서버와 브라우저는 기본적으로 HTTP 메시지 안에 이 쿠키를 담아서 주고 받게 됩니다

![image](https://github.com/user-attachments/assets/f890182e-d213-400e-9738-78ceb4d79b12)


### **Cookie (요청 헤더,Request Header)**

- HTTP 요청 시 클라이언트에서 서버로 전달하는 쿠키 헤더

- 서버의 세션 상태를 담은 SessionID 혹은 각종 클라이언트 정보들을 담고 있다.

- `Cookie` 요청 헤더에는 아래와 같은 형식에 따라 여러 개의 쿠키를 `;`로 구분하여 나열할 수 있습니다.

- ```bash
  Cookie: name=value
  Cookie: name=value; name2=value2; name3=value3
  
  example
  Cookie: PHPSESSID=298zf09hf012fh2; csrftoken=u32t4o3tb3gg43; _gat=1
  
  ```

---



### **Set-Cookie (응답 헤더,Response Header)**

- 서버는 `Set-Cookie`라는 응답 헤더에 브라우저가 수신해야 할 쿠키 정보를 명시하도록 되어 있습니다.

- 하나의 `Set-Cookie` 응답 헤더에는 하나의 쿠키만 담을 수 있어서 여러 개의 쿠키를 보낼 때는 아래와 같은 모습이 됩니다.

  - 여러개의 value를 콤마(,)로 열거하여 저장해 구분합니다.

- ```bash
  Set-Cookie: <cookie-name>=<cookie-value>
  Set-Cookie: <cookie-name>=<cookie-value>; Domain=<domain-value>
  Set-Cookie: <cookie-name>=<cookie-value>; Expires=<date>
  Set-Cookie: <cookie-name>=<cookie-value>; HttpOnly
  Set-Cookie: <cookie-name>=<cookie-value>; Max-Age=<number>
  Set-Cookie: <cookie-name>=<cookie-value>; Partitioned
  Set-Cookie: <cookie-name>=<cookie-value>; Path=<path-value>
  Set-Cookie: <cookie-name>=<cookie-value>; Secure
  
  Set-Cookie: <cookie-name>=<cookie-value>; SameSite=Strict
  Set-Cookie: <cookie-name>=<cookie-value>; SameSite=Lax
  Set-Cookie: <cookie-name>=<cookie-value>; SameSite=None; Secure
  
  // Multiple attributes are also possible, for example:
  Set-Cookie: <cookie-name>=<cookie-value>; Domain=<domain-value>; Secure; 
  ```

- 클라이언트는 이 정보를 바탕으로 쿠키를 만들어 브라우저에 저장합니다.

![image](https://github.com/user-attachments/assets/98c5c960-a09f-432e-88bc-8e24cf3de8c0)


set-cookie 속성들

- 아래 링크에서 자세하게 확인이 가능합니다.

- ## [Attributes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie#attributes)


---

### 쿠키의 적용범위

브라우저가 기본적으로 쿠키를 보낸 서버가 속한 도메인으로만 쿠키를 되돌려 보내지만 어떤 URL을 방문할 때 해당 쿠키를 보내야할지 말지를 좀 더 세밀하게 제어할 수도 있습니다.

`Set-Cookie` 응답 헤더에 `Domain` 속성을 명시하면, 서브 도메인까지 포함되도록 쿠키의 범위가 확장됩니다.

- HTTP응답

```bash
Set-Cookie: <쿠키 이름>=<쿠키 값>; Domain=도메인
```

예를 들어, `Domain` 속성을 `test.com`으로 설정하면 브라우저는 `a.test.com`으로 부터 받은 쿠키를, `b.test.com`으로도 보내게 됩니다. 

그러므로 `a.test.com`과 `b.test.com`이 쿠키를 공유하는 효과가 발생하게 됩니다.

`Set-Cookie` 응답 헤더에 `Path` 속성을 명시하면 쿠키의 범위를 해당 도메인의 특정 경로로 쿠키의 범위를 축소시킬 수 있습니다.

- HTTP응답

```bash
Set-Cookie: <쿠키 이름>=<쿠키 값>; Path=경로
```

예를 들어, `Path` 속성이 `/users`라고 설정되어 있는 쿠키는, 브라우저가 `/users`를 포함한 하위 경로로 요청을 할 때만 서버로 돌려 보냅니다.

---

## 쿠키의 유효 기간

쿠키의 유효기간은 서버가 맨 처음 쿠키를 보낼 때 결정을 하며, `Set-Cookie` 응답 헤더를 통해 명시되야 합니다.

유효 기간이 별도로 명시되지 않은 쿠키를 보통 세션 쿠키(session cookie)라고 부르는데 브라우저의 세션이 종료될 때 함께 만료됩니다. 

즉, 브라우저의 탭이나 윈도우를 닫으면 서버가 보냈던 쿠키는 모두 만료되어 브라우저는 더 이상 해당 쿠키를 서버에 돌려 보내지 않습니다.

반면에 유효 기간이 명시되어 있는 쿠키인 영속 쿠키(permanent cookie)는 세션과 무방하게 특정 기간이나 특정 시점까지 유효합니다. 

쿠키의 유효 기간을 명시할 때는 `Set-Cookie` 응답 헤더에 `Expires` 속성이나 `Max-Age` 속성을 다음과 같은 형태로 사용합니다.

- HTTP응답

```bash
Set-Cookie: <쿠키 이름>=<쿠키 값>; Expires=종료 시점
Set-Cookie: <쿠키 이름>=<쿠키 값>; Max-Age=유효 기간
```

`Max-Age` 속성은 초 단위로 설정하며, `Expires` 속성과 `Max-Age` 속성이 둘 다 있을 경우 `Max-Age` 속성이 우선합니다. 

**두 가지 속성이 모두 없을 때 해당 쿠키는 세션이 종료될 때 만료되는 세션 쿠키가 됩니다.**

참고로 브라우저에 저장해두었던 쿠키를 즉시 만료하고 싶을 때는 다음과 같이 `Max-Age` 속성을 `0`으로 설정해주면 되겠습니다.

- HTTP응답

```bash
Set-Cookie: a=1; Max-Age=0
```





