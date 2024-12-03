# Restful API HATEOAS

- **HATEOAS (Hypermidia As The Engine Of Application State)**

  - 단어의 정의
    - 하이퍼미디어를 애플리케이션의 상태를 관리하기 위한 메커니즘으로 사용한다는 것
  - 좀 더 이해하기 쉬운 정의
    - **응답에 데이터 뿐만 아니라 해당 데이터와 관련된 요청에 필요한 URI를 응답에 포함하여 반환**합니다
    - **이를 통해 api를 사용하는 클라이언트가 전적으로 서버와동적인 상호작용이 가능하도록 해줍**니다.

- **Rest API의 구현단계**

  - 아래 그림으로 확인할 수있습니다

![image](https://github.com/user-attachments/assets/0b75ac4c-bb1c-4a32-bb2f-3170851b6bc5)


![image](https://github.com/user-attachments/assets/4e6d01ed-1e4d-40fb-a0d0-6c6f92372c18)


- 레벨 0
  - **HTTP 프로토콜을 사용하여 API를 구현하지만 모든 기능을 활용하지는 않는다. 또한 리소스에 대한 고유 주소는 제공**되지 않는다.
- 레벨 1
  - **리소스에 대한 고유 식별자가 있지만 리소스에 대한 각 작업에는 고유한 URL이 있다.**
- 레벨 2
  - **동작을 설명하는 동사 대신 HTTP 메소드를 사용한다.**
- 레벨 3
  - **HATEOAS가 도입되었다.**
  - 리소스에 하이퍼미디어를 도입하며, 이를 통해 가능한 작업에 대해 알려주는 응답에 링크를 배치할 수 있으므로 API를 통해 탐색할 수 있는 가능성이 추가된다.

### HAL

- Hypertext Application Language으로 JSON, XML 코드 내의 외부 리소스에 대한 링크를 추가하기 위한 특별한 데이터 타입이다.
- 이 HAL 타입을 이용한다면 쉽게 HATEOAS를 달성할 수 있으며, 두 가지의 특징만 이용하면 된다.

1. 리소스 : 일반적인 데이터 필드에 해당한다.
2. 링크 : 하이퍼미디어로 보통 _self 필드가 링크 필드가 된다.

**application/hal+json HATEOAS 응답 형태**

```json
{
    ~리소스 영역~
    "id": 100,
    "subject": "게시글 100",
    "content": "내용"
    ~리소스 영역~

    ~링크 영역~
    "_links" : {
        "self": {
            "href": "<https://my-test-server.com/api/articles/100>"
        },
        "next": {
            "href": "<https://my-test-server.com/api/articles/101>"
        },
        "like": {
            "href": "<https://my-test-server.com/api/articles/likes>"
        },
        "comment": {
            "href": "<https://my-test-server.com/api/articles/100/comments>"
        },
        "home": {
            "href": "<https://my-test-server.com/>"
        }
    }
    ~링크 영역~
}
```

- **HATEOAS를 사용하여 얻게 되는 가장 큰 장점은 서버와 클라이언트를 분리한다는 것입니다**.
- 클라이언트는 서버의 상태와 행동을 알 필요가 없으며 서버 측의 변경에도 클라이언트에선 일일이 대응하지 않아도 됩니다.

즉 HATEOAS가 적용된 level 3의 REST API는 응답에 대한 데이터 뿐만 아니라 해당 데이터와 관련된요청의 url을 hal 형태로 추가하여 클라이언트에게 응답합니다
