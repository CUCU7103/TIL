# HTTP Method

### HTTP Method란 클라이언트와 서버 사이에 이루어지는 요청(request), 응답(response) 데이터를 전송하는 방식입니다.

> [!NOTE]
>
> 주요 메소드
>
> GET : 리소스 조회
> POST:  요청 데이터 처리, 주로 등록에 사용
> PUT : 리소스를 대체(덮어쓰기), 해당 리소스가 없으면 생성
> PATCH : 리소스 부분 변경 (PUT이 전체 변경, PATCH는 일부 변경)
> DELETE : 리소스 삭제
>
> 기타 메소드
>
> HEAD : GET과 동일하지만 메시지 부분(body 부분)을 제외하고, 상태 줄과 헤더만 반환
> OPTIONS : 대상 리소스에 대한 통신 가능 옵션(메서드)을 설명(주로 CORS에서 사용)
> CONNECT : 대상 자원으로 식별되는 서버에 대한 터널을 설정
> TRACE : 대상 리소스에 대한 경로를 따라 메시지 루프백 테스트를 수행



## GET method

- 리소스를 조회하는 메서드 입니다.

  -  데이터를 읽거나(Read) 검색(Retrieve)할 때에 사용되는 메소드

- GET 요청은 멱등성이다.

- GET 요청에서 서버에 데이터를 전달하는 경우 쿼리스트링을 통해서 데이터를 전달합니다.

- GET은 캐싱이 가능하여 같은 데이터를 한번 더 조회할 경우에 저장한 값을 사용해 조회속도를 보장합니다.

- **정적 데이터 조회 과정**

  - 이미지, 정적 텍스트 문서 GET
  - 쿼리 파라미터 없이 리소스 경로로 단순하게 조회 가능

  

- **동적 데이터 조회 과정**

  - 주로 검색, 게시판 목록에서 검색어로 이용

  - 쿼리 파라미터 사용해서 데이터를 전달

  - 쿼리 파라미터는 key1=value1&key2=value2 구조로 되어 있음



## **POST method**

- **전달한 데이터 처리/생성 요청 메서드 (Create)**
- 메시지 바디(body)를 통해 서버로 요청 데이터 전달하면 서버는 요청 데이터를 처리하여 업데이트
- 전달된 데이터로 주로 신규 리소스 등록, 프로세스 처리에 사용
  - 데이터를 **메세지 바디에 쿼리 파라미터 형식으로 전달**합니다.
    - 쿼리 파라미터는 key - value 형식으로 되어 있습니다.
    - 이는 GET 방식과 비교하면, 데이터가 외부로 노출되지 않으므로, 보안상의 이점이 있습니다.
- 만일 데이터를 GET 하는데 있어, JSON으로 조회 데이터를 넘겨야 하는 애매한 경우 POST를 사용



## **PUT method**

- **리소스를 대체(수정)하는 메서드 (Update)**
- **URI에 작업대상 리소스의 경로를 명시합니다.**
- **만일 요청 메세지에 리소스가 있으면 덮어쓰고, 없으면 새로 생성한다**.
  - /members/100 데이터가 존재하면 기존에 것을 완전 대체 한다.
  - /members/100 데이터가 없으면 대체 할게 없으니까 새로 생성한다.

- 데이터를 대체해야 하니, 클라이언트가 리소스의 구체적인 전체 경로를 지정해 보내주어야 한다.
  - POST /members : 멤버 새로 추가
  - PUT /members/100 : 100번째 멤버 수정

- PUT 요청에 해당하는 리소스가 있는 경우

  ![image-20241121004327177](https://raw.githubusercontent.com/CUCU7103/typora_images/main/image/image-20241121004327177.png?token=AZT7RR6MGBY5OZB6P26XMYDHHYB52)

  ![image-20241121004344042](https://raw.githubusercontent.com/CUCU7103/typora_images/main/image/image-20241121004344042.png?token=AZT7RR5UEP6USGOOMD4PVF3HHYB62)

  - 기존에 있는 데이터를 완전히 대체한다.

    

- PUT 요청에 해당하는 리소스가 없는 경우

  ![image-20241121004411232](https://raw.githubusercontent.com/CUCU7103/typora_images/main/image/image-20241121004411232.png?token=AZT7RR4T45B6GNL27Q7QVJTHHYCAQ)

  ![image-20241121004423915](https://raw.githubusercontent.com/CUCU7103/typora_images/main/image/image-20241121004423915.png?token=AZT7RR44MSM4USZWRPZYTBDHHYCBK)

  - 기존의 데이터가 없다면 POST와 같이 신규로 생성한다.

- PUT 요청에 일부 리소스만 변경하길 원하는 경우?

  - ![image-20241121004536660](https://raw.githubusercontent.com/CUCU7103/typora_images/main/image/image-20241121004536660.png?token=AZT7RR4M77RXAWDZIH2SFNLHHYCF4)
  - ![image-20241121004548071](https://raw.githubusercontent.com/CUCU7103/typora_images/main/image/image-20241121004548071.png?token=AZT7RR3KAAK6J2QKVEENC7LHHYCGS)
  - 기존 데이터가 완전히 대체되어집니다. 즉 위의 예시에서는 useraname 데이터가 삭제되어 집니다.

## **PATCH **

- **리소스 일부 부분을 변경하는 메소드** **(Update)**

- 만일 PATCH를 지원하지 않는 서버에서는 대신에 POST를 사용할 수 있다.

- Petch 요청에서 일부 리소스만 변경하려고 할때

  ![image-20241121004720850](https://raw.githubusercontent.com/CUCU7103/typora_images/main/image/image-20241121004720850.png?token=AZT7RRYMASFJF2GVIRRXW43HHYCMM)

![image-20241121004729595](https://raw.githubusercontent.com/CUCU7103/typora_images/main/image/image-20241121004729595.png?token=AZT7RR7QP42MMOYUTY56FG3HHYCM6)

- PUT과는 다르게 기존 리소스는 유지하면서 변경 요청한 부분만 수정되어진다.

## **DELETE method**

- **리소스 제거하는 메소드 (Delete)**
- 상태코드는 대부분 200을 사용하고 상황에 따라 204를 사용한다.

## HEAD method

- GET과 동일하지만 서버에서 Body를 Return 하지 않습니다.
- 응답의 상태 코드만 확인할때와 같이 Resource를 받지 않고 오직 찾기만 원할때 사용 (일종의 검사 용도)
- 서버의 응답 헤더를 봄으로써 Resource가 수정 되었는지 확인 가능

![image-20241121004933435](https://raw.githubusercontent.com/CUCU7103/typora_images/main/image/image-20241121004933435.png?token=AZT7RRZRF4DKFF6KKUKSZT3HHYCUW)



## **TRACE method**

- 일종의 검사용 메서드이다.
- 서버에 도달 했을 때의 최종 패킷의 요청 패킷 내용을 응답 받을 수 있다.
- 요청의 최종 수신자는 반드시 송신자에게 200(OK) 응답의 내용(Body)로 수신한 메세지를 반송해야 한다.
- 최초 Client의 요청에는 Body가 포함될 수 없다.

![image-20241121005054232](https://raw.githubusercontent.com/CUCU7103/typora_images/main/image/image-20241121005054232.png?token=AZT7RR73YWKX244HIRUHCYDHHYCZY)



## **OPTION**

- 예비 요청(Preflight)에 사용되는 HTTP 메소드
- 예비 요청이란?
  - 본 요청을 하기 전에 해당 경로가 안전한지 미리 검사하는 것입니다.
- 서버의 지원 가능한 HTTP 메서드와 출처를 응답 받아 [CORS 정책Visit Website](https://inpa.tistory.com/entry/WEB-📚-CORS-💯-정리-해결-방법-👏)을 검사하기 위한 요청이다.



![image-20241121005229537](https://raw.githubusercontent.com/CUCU7103/typora_images/main/image/image-20241121005229537.png?token=AZT7RRYQDHGMKV74TEIIDO3HHYC7W)



# 멱등성

> [!NOTE]
>
> **동일한 요청을 여러 번 수행해도 서버의 상태나 응답이 동일하게 유지되는 특성을 말합니다**
>
> **즉 , 멱등한 작업의 결과는 한 번 수행하든 여러 번 수행하든 같습니다.**
>
> 멱등성에서의 상태
>
> - 서버가 특정 시점에 보유하고 있는 데이터나, 리소스의 현재 상태를 의미합니다.

![image-20241121010839433](https://raw.githubusercontent.com/CUCU7103/typora_images/main/image/image-20241121010839433.png?token=AZT7RR7AQ2FHGCG4MVBM33DHHYE4I)





### **PUT 메서드 (멱등함)**

**설명:** 

- 리소스를 전체적으로 대체할 때 사용됩니다. 
- 여러번 호출해도 매번 같은 리소스로 업데이트 되기 때문에 결과가 달라지지 않습니다.

- **같은 데이터를 여러 번 PUT 요청으로 보내도 서버의 상태는 동일하게 유지됩니다.**



### **POST 메서드 (멱등하지 않음)**

**설명:** 

- 서버에 새로운 리소스를 생성하거나, 서버 상태를 변경하는 작업에 사용됩니다. 

- **여러 번 호출하면 보내면 리소스가 중복 생성**되거나 서버의 상태에 변화를 일으킬 수있습니다.



멱등성의 관점에서 두 메서드를 비교해보면 결국 가장 명확한 차이는 리소스의 중복생성입니다.

Post의 경우 요청을 보낼때마다 리소스를 생성하고 이는 중복되는 리소스를 만들수 있기에 서버의 상태를 변화시킬 수있습니다.

Put의 경우 새로운 리소스를 생성하거나, 리소스가 존재한다면 데이터를 덮어쓰기합니다.

즉 여러번의 요청을 보낸다고해도 리소스를 계속 업데이트 하기에 서버의 상태에 변화를 주지 않습니다.




