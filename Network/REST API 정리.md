# RESTful API

- REST([Representational State Transfer](https://www.ibm.com/kr-ko/topics/api)) 아키텍처 스타일의 설계 원칙을 준수하는 *API(**애플리케이션프로그래밍 인터페이스)*입니다.

- 요청 메시지만 보고도 무엇을 원하는지 파악이 가능하다.

  

## HTTP 프로토콜이란?

- 클라이언트와 서버간의 데이터를 주고받기 위해 사용되는 표준

 ![image](https://github.com/user-attachments/assets/447fd966-224e-4f7a-9d2f-9e0203f5a99d)


- **HTTP 프로토콜이 클라이언트와 서버간의 데이터를 주고 받는 표준이 되어있긴 하지만 이를 구현하는 방식은 유연하기때문에 개발자마다 어떻게 구현할지에 대한 방식이 달라지게 됩니다**.

 ![image](https://github.com/user-attachments/assets/bcc67df7-8a1d-40c5-a7f0-d81ee8a11597)


- 즉 서버와 통신하는 클라이언트가 각기 다른 방식으로 HTTP 프로토콜을 구현하게 된다면 이는 유지보수가 매우 어려워집니다.

  

## REST란?

![image](https://github.com/user-attachments/assets/5b2b42b1-77d4-43e6-8a5b-d0c860274712)


- ### REST(Representational State Transfer)의 약자로 자원을 이름(표현)으로 구분하여 해당 자원의 상태를 주고받는 모든 것을 의미합니다.

  - HTTP URI를 통해 자원을 이름(표현)으로 명시하고
  - HTTP Method(POST, GET, PUT, DELETE, PATCH 등)를 통해 해당 자원(URI)에 대한 CRUD Operation을 적용하는 것을 의미합니다.

---



> [!TIP]
>
> ## **CRUD Operation**
>
> CRUD는 대부분의 컴퓨터 소프트웨어가 가지는 기본적인 데이터 처리 기능인 Create(생성), Read(읽기), Update(갱신), Delete(삭제)를 묶어서 일컫는 말로 REST에서의 CRUD Operation 동작 예시는 다음과 같다.
>
> - Create : 데이터 생성(POST)
> - Read : 데이터 조회(GET)
> - Update : 데이터 수정(PUT, PATCH)
> - Delete : 데이터 삭제(DELETE)

---



# **REST 구성 요소**

REST는 다음과 같은 3가지로 구성이 되어있습니다.

1. **자원(Resource) : HTTP URI**
2. **자원에 대한 행위(Verb) : HTTP Method**
3. **자원에 대한 행위의 내용 (Representations) : HTTP Message Pay Load**

![image](https://github.com/user-attachments/assets/884356d8-ba54-4c87-af36-29df781b1a06)


# REST-API 구현

> [!NOTE]
>
> ## 자원
>
> - #### 모든 URI는 자원으로 표현한다.
>
>  ![image](https://github.com/user-attachments/assets/2d79a51c-b93c-49f9-8152-d91e812971dd)

>
> - #### 슬래시 구분자를 통해 자원 간의 계층 관계를 나타낸다.
>
>   ![image](https://github.com/user-attachments/assets/55b7ec3c-ec6e-44fc-b972-d3cbc6164703)

>
> - #### 언더바 대신  하이픈을 사용해야 한다.
>
>  ![image](https://github.com/user-attachments/assets/7125aa13-f3df-48b4-879d-8f77debe2360)

>
> - #### 소문자를 사용해야 한다.
>
>  ![image](https://github.com/user-attachments/assets/bb285831-9597-4d52-b5c0-520e4bd2b2a8)
>
> - #### URI의 마지막에는 슬래시를 포함하지 않습니다.
>
>   ![image](https://github.com/user-attachments/assets/2468a5d4-663a-4408-a92b-40402f7e6bca)




> [!NOTE]
>
> ## 행위(상태) - HTTP Method
>
> - 자원에 대한 모든 행위는 HTTP Method로 표현한다.
>
>   





> [!NOTE]
>
> ### 표현 - Request Header
>
> - 리소스의 응답 타입은 요청 헤더로 나타냅니다.
>
> - Accept 헤더를 사용하여 응답의 타입을 지정함
>
>   ![image](https://github.com/user-attachments/assets/9c0f6331-df7e-4312-86af-3bbdc53a5804)

>
> - 즉 클라이언트는 원하는 자원과 그 표현을 요청하고 서버는 해당하는 자원과 표현방법에 맞춰 응답한다.
>
>   ![image](https://github.com/user-attachments/assets/43246f95-f2c1-41a9-9915-ad6129401b94)






# REST-API의 설계원칙

### <u>균일한 인터페이스</u> 

### 클라이언트 - 서버 구조

### 무상태성

### 캐시가능성

### 계층형 시스템

### 주문형 코드(Optional)

### 



---

## <u>균일한 인터페이스</u> (가장 중요하다.)

- **개발을 할때 클라이언트와 맞닿아 있는 부분(Http Request, Http Response 등)을 쉽고 일반적으로 설계하라는 제약조건입니다.**
  - 동일한 리소스에 대한 모든 API 요청은 요청의 출처에 관계없이 동일하게 표시되어야 합니다.
  - REST API는 사용자의 이름 또는 이메일 주소와 같은 동일한 데이터가 하나의 통합 리소스 식별자(URI)에만 속하도록 해야 합니다.
  - 리소스가 너무 커서는 안 되며 클라이언트가 필요로 할 수 있는 모든 정보를 포함해야 합니다.



## 클라이언트-서버 구조

- **REST API 설계에서 클라이언트 및 서버 애플리케이션은 서로 완전히 독립적이어야 합니다.**
  - **클라이언트(사용자 인터페이스)와 서버(데이터 저장 및 처리)가 독립적으로 동작해야 한다는 원칙입니다.**

- **클라이언트 애플리케이션이 알아야 하는 유일한 정보는 요청된 리소스의 URI입니다.**
- 서버 애플리케이션과 다른 방법으로 통신할 수 없습니다.
- 마찬가지로 서버 애플리케이션은 HTTP를 통해 요청된 데이터에 클라이언트 애플리케이션을 전달하는 것 외에는 클라이언트 애플리케이션을 수정해서는 안 됩니다.

![image](https://github.com/user-attachments/assets/2dad9c44-871f-4d86-8f68-85085a1dbd1f)




## 무상태성

- 요청은 상태를 가지지 않는다는 제약 조건입니다.
- 각각의 요청은 독립적이고 필요한 모든 정보를 제공해야 합니다.
  - REST API는 무상태성이기 때문에 각 **요청에는 처리에 필요한 모든 정보가 포함되어야** 합니다.
    - **무상태는 서버가 클라이언트의 상태를 보존하지 않는 것을 의미**합니다.
    - **서버는 단순히 요청에 해당하는 응답을 보내는 역할만 수행하며, 통신에 필요한 모든 상태 정보는 클라이언트에서 제공하는 것**
  - ![image](https://github.com/user-attachments/assets/d3090bb0-0b21-4f0b-b490-f2fd3cbd9f0b)





## 캐시가능성

- 서버는 자원이 캐시 가능한지 명시해야 한다는 제약조건 입니다.
  - 가능한 경우 클라이언트나 서버 측에서 리소스를 캐시할 수 있어야 합니다
  - **데이터를 임시로 저장(캐시)하여 성능을 향상시키고 서버 부하를 줄일 수 있어야 한다는 원칙입니다**

![image](https://github.com/user-attachments/assets/6fde8024-9ccd-486e-8a5d-fcc5de58dfc5)




## 계층형 시스템

- 계층형 시스템을 적용해야 한다는 제약조건
- 클라이언트가 한번에 여러가지 요청을 보냈을때 서버가 각각의 책임에 맞게 요청을 처리할 수있도록 구성해야함.

![image](https://github.com/user-attachments/assets/e22d1660-95cc-423b-adcf-985731174ede)




## 주문형 코드

- 클라이언트가 필요에 의해 기능을 확장할 수 있도록 해야한다는 제약조건
  - 대표적인 예시로는 플러그인이 존재한다.

![image](https://github.com/user-attachments/assets/ae9f84ae-f51f-475f-a1ab-42d8e26ed601)































