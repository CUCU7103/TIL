# Filter vs interceptor vs AOP

- 웹 개발을 하다보면 공통적으로 처리해야 할 업무들이 많습니다.
  - 예를 들자면 , 로그인 관련 처리, 권한 체크, XSS 방어, pc 와 모바일 웹 등의 분기처리 등이 있습니다.
  - 이러한 공통되어지는 업무를 "공통 관심사"라고 하며 이러한 공통관심사를 분리하는 것이 중복사항을 제거하고 객체지향적으로 코드를 작성할 수 있으며 유지보수가 용이해집니다.



![image-20241212235647517](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20241212235647517.png)



**Filter**

- filter는 서블릿 컨테이너에서 DispatcherServlet 이전에 실행이 되어집니다.
- DispathcherServlet 이전에 http요청/응답 흐름 전체에 관여할 수 있습니다.
- 모든 URL 패턴에 대해서 동작할 수있으며 스프링 프레임워크의 존재와는 무관하게 작동이 가능합니다.

- Filter는 FilterChain을 통해 여러 필터가 연쇄적으로 동작하게 할 수 있다.
- ServletRequest 혹은 ServletResponse를 교체할 수 있다.



**Interceptor** 

- **DispatcherServlet이 핸들러 목록에서 핸들러 매핑을 하고난 뒤 컨트롤러를 호출하기 전/후 요청에대해 부가적인 작업을 처리하는 객체입니다.**
- **DispatcherServlet과 Handler(Controller)사이에서 전/ 후로 인터셉터 말 그대로 낚아챌 수있습니다.**
- **Filter와의 차이점은 Intercepter가 스프링의 기술이기 때문에 스프링에서 관리하는 빈들을 사용할 수 있다.**
- **스프링 MVC 환경 내에서만 유효하며, HandlerMapping 에 의해 선택된 Handler(Controller) 전/후에 개입하는 형태로 동작한다.**
- 애플리케이션 레벨에서 세분화된 로직에 좀 더 사용하기 적합합니다.
- 공통 비즈니스 로직들을 처리하는데 적합합니다.



### AOP

- AOP는 OOP를 보완하기 위해 나온 개념으로 관점 지향 프로그래밍이라 불립니다. 

- 객체 지향 프로그래밍을 했을 때 중복을 줄일수 없는 부분을 줄이기 위해 종단면에서 바라보고 처리한다.

- 주로 로깅, 트랜잭션, 에러처리에 사용되며, **Filter와 Intercepter와 다르게 비즈니스 로직을 처리할 때 사용된다.** 메소드 전 후로 자유롭게 설정할 수 있다.

- **InterCepter, Filter는 주소로 대상을 구분해서 걸러내야하는 반면, AOP는 주소, 파라미터, 애노테이션 등 다양한 방법으로 대상을 지정할 수 있다.**

- **AOP의 대상 지정 방법** 

  AOP는 포인트컷(Pointcut) 표현식을 사용하여 더 세밀하게 대상을 지정합니다

  - 메소드 이름, 클래스 이름, 패키지 이름, 메소드의 매개변수 등을 기반으로 조인포인트를 선택할 수 있습니다.
  - AspectJ 표현식을 사용하여 더 복잡한 조건을 설정할 수 있습니다



