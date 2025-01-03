## AOP



### Spring AOP란?

- Spring AOP는 스프링 프레임워크에서 제공하는 기능 중 하나로 관점 지향 프로그래밍을 지원하는 기술입니다. 
- Spring AOP는 로깅, 보안, 트랜잭션 관리 등과 같은 공통적인 관심사를 모듈화 하여 코드 중복을 줄이고 유지 보수성을 향상하는데 도움을 줍니다.

### AOP란?

- Aspect Oriented Programming의 약자로 '관점 지향 프로그래밍을 의미합니다.'
- **핵심적인 비즈니스 로직으로 부터 횡단관심사를 분리하는 것에 목적**을 둡니다.
- 객체 지향 프로그래밍 패러다임을 보완하는 기술로 **<u>메소드나 객체의 기능을 핵심 관심사(Core Concern)와 공통 관심사(Cross-cutting Concern)로 나누어 프로그래밍하는 것</u>**
  - **“핵심 관심사”는 각 객체가 가져야 할 본래의 기능**이며, **“공통 관심사”는 여러 객체에서 공통적으로 사용되는 코드**를 말합니다.
  - 즉 부가기능을 따로 관리하는 것을 의미합니다.

![image-20241211224159577](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20241211224159577.png)

![image-20241211215709790](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20241211215709790.png)

횡단 관심사 예시

- 비즈니스 로직의 앞 뒤에 시작시간과 끝나는 시간을 체크하여 실행시간을 출력하기

  - 이 비즈니스 로직은 카트에 사용자의 물건을 넣는 것이다.
  - 시간을 체크하는 것은 이 로직의 주요한 기능이 아닙니다.

  ![image-20241211220046690](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20241211220046690.png)

-  그런데 이러한 기능이 이 비즈니스 로직, 메서드말고 다른 모든 로직에 적용되야 한다면 ?

  - 이러한 부가기능들을 한번에 관리할 수 있는 방법이 없을까?

- ### 이럴 때 AOP를 사용하여 횡단관심사를 처리할 수 있다



### AOP의 장점

- **전체 코드 기반에 흩어져 있는 관심 사항이 하나의 장소로 응집**합니다.

  - 중복사항이 제거된다.

- 자신의 주요 관심사항에 대한 코드만 포함하고 있기에 코드가 깔끔해집니다.

- 객체지향적으로 코드를 작성할 수있게 되고, 유지보수가 용이해 집니다.

  

### AOP의 용어

1. Target
   - 부가기능을 부여할 대상 (핵심기능을 담고 있는 모듈)

2. Aspect
   - **부가기능 모듈을 Aspect라고 부른다. (핵심기능에 부가되어 의미를 갖는 모듈**)
   - **부가될 기능을 정의한 Advice와 Advice를 어디에 적용할지를 결정하는 PointCut을 함께 갖고 있다.**
   -  **어플리케이션의 핵심적인 기능에서, 부가적인 기능을 분리해서 Aspect라는 모듈로 만들어서 설계하고 개발하는 방법**

3. Advice 
   - 실질적으로 부가기능을 담은 구현체
   - 타겟 오브젝트에 종속되지 않기 때문에, 부가기능에만 집중할 수 있음
   - Aspect가 무엇을 언제 할지를 정의

4. PointCut
   - 부가기능이 적용될 대상(Method)을 선정하는 방법
   - Advice를 적용할 JoinPoint를 선별하는 기능을 정의한 모듈

5. JoinPoint
   - Advice가 적용될 수 있는 위치
   - Spring에서는 메소드 조인포인트만 제공한다.
   -  타겟 객체가 구현한 모든 메소드는 조인 포인트가 된다.
   -  join point에는 아래와 같은 6가지 시점이 존재합니다.

  ![image](https://github.com/user-attachments/assets/c4f142d4-8ab5-42b9-832e-e4fd30869607)
  

6. Proxy
   - Target을 감싸서 Target의 요청을 대신 받아주는 랩핑 오브젝트.
   - 클라이언트에서 Target을 호출하게되면, 타겟이 아닌 타겟을 감싸고 있는 Proxy가 호출되어,
   - 타겟메소드 실행 전에 선처리, 후처리를 실행한다.

7. Introduction
   - 타겟 클래스에 코드변경없이 신규메소드나 멤버변수를 추가하는 기능

8. Weaving

   - 지정된 객체에 Aspect를 적용해서, 새로운 프록시 객체를 생성하는 과정

   - Spring AOP는 런타임에서 프록시 객체가 생성된다.

 

## 스프링 AOP 동작과정

> [!TIP]
>
> ### BeanPostProcessor 
>
> ![image-20241211221659839](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20241211221659839.png)
>
> - 생성된 빈 객체를 스프링 컨테이너에 동작하기 전에 조작하는 객체
>
> 
>
> 역할
>
> - 생성된 빈 전달
> - 빈 교체 대상 확인
> - 빈 교체 대상이 맞다면 이를 조작 또는 교체한다.
> - 교체하거나 전달받은 빈을 확인한다
> - 반환된 빈 객체를 컨텍스트에 등록합니다.



> [!TIP]
>
> ### Proxy
>
> ![image-20241211221956790](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20241211221956790.png)
>
> - 원본 객체를 감싼 새로운 객체로, 스프링 AOP에서 부가기능을 추가할 때 사용하는 패턴
>
>   



![image-20241211221823279](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20241211221823279.png)

1) Bean 객체를 생성한 뒤 빈 후처리기(BeanPostProcessor)에게 전달합니다.

2. 어드바이저 내의 포인트 컷을 이용해 전달받은 빈이 프록시 적용 대상인지 확인합니다.
3. 프록시 생성 대상 bean들을 대상으로 프록시 객체를 생성합니다.
4. 프록시를 생성한 빈이라면 프록시 객체를, 프록시를 생성하지 않은 빈 이라면 빈을 반환한다.
5. 빈 후처리기(BeanPostProcessor)에서 전달받은 객체를 컨테이너의 빈으로 등록한다.















