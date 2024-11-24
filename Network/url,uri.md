![image](https://github.com/user-attachments/assets/d61de3dc-9fe1-4f9d-bda2-87bc4453a86f)

### URI - 자원의 식별자

![image](https://github.com/user-attachments/assets/3c36c837-b6fc-4258-bc74-aa04246ccf18)


- 인터넷에 있는 자원을 어디에 있는지 자원 자체를 식별하는 방법

- **URI는 그 자체로 이름이 될 수 있다.**

- Uniform : 

  - 리소스를 식별하는 통일된 방식

- Resource : 

  - 자원, URI로 식별할 수 있는 모든 것여기서 자원은 웹 브라우저의 파일만 뜻하는 게 아니라, 실시간 교통정보 등 우리가 구분할 수 있는 것은 모든 게 리소스가 된다.

- Identifier : 

  - 다른 항목과 구분하는데 필요한 정보

- elancer.co.kr > **URI**

- https://www.elancer.co.kr > **URL, URI**

  

### URL - 위치(Location)

![image](https://github.com/user-attachments/assets/e1d1f8e8-6ab2-4eb4-aa2f-da96dcc14f86)


-  네트워크 상에서 자원이 어디 있는지 위치를 알려주기 위한 규약이다.

-  **URL은 프로토콜과 결합한 형태**

-  https://www.elancer.co.kr > **URL**

   

- URI와 URL를 쉽게 구분하는 방법은
- URI 는 통합 자원 식별자로 주소에 식별자가 있으면 URI
- URL은 리소스 주소를 나타내므로 리소스 위치까지만 나타내면 URL 입니다.



다음 예시를 통해 비교하며 이해해봅시다.

1. ```https://hstory0208.tistory.com/category```

**hstory0208.tistory.com 에서 category 라는 경로를 나타냅니다.**
category는 리소스의 실제 위치이므로 이 주소는 URL 입니다.

2. ```https://hstory0208.tistory.com/category/12```

**hstory0208.tistory.com 에서 category 라는 자원의 경로를 나타내는 부분까진 URL 이**지만
/12 는 식별자 이므로 ```https://hstory0208.tistory.com/category``` URL을 포함한 URI라고 할 수 있습니다.

3. ```https://hstory0208.tistory.com/category?page=12```

위와 마찬가지로 ```https://hstory0208.tistory.com/category ```까지는 자원의 실제 위치를 나타내기 때문에 URL이고, 뒤의 query ( ?page=12 ) 가 붙었으므로 https://hstory0208.tistory.com/category URL을  포함한 URI 입니다.

