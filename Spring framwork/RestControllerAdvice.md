# @RestControllerAdvice

- ### spring MVC에서 제공하는 어노테이션으로 @ControllerAdvice와  @ResponseBody가 합쳐진 형태입니다.

- @ControllerAdvice가 전체 컨트롤러 계층에 걸쳐 공통적으로 적용될 로직(주로 예외 처리, 바인딩 설정 등)을 모아두는 역할입니다

- @RestControllerAdvice는 그 안에서 **발생하는 반환 결과에 기본적으로 @ResponseBody를 적용해 REST 스타일로 응답을 내려주는 것에 최적화되어 있습니다.**

```  java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(IllegalArgumentException.class)
    public ResponseEntity<String> handleIllegalArgumentException(IllegalArgumentException ex) {
        return ResponseEntity.badRequest().body("잘못된 요청입니다: " + ex.getMessage());
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<String> handleException(Exception ex) {
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR)
                             .body("서버 오류가 발생했습니다: " + ex.getMessage());
    }
}

```

- `@ExceptionHandler`를 통해 특정 예외 유형을 잡아 처리할 수 있고,

- 결과를 `ResponseEntity` 또는 원하는 형식(예: JSON)으로 만들어 응답합니다.

- 모든 컨트롤러에 대해 이 예외 처리 로직이 **자동으로 적용**됩니다.

### AOP와의 관계

**공통 기능(관심사) 분리**

- AOP의 핵심 개념인 **관심사의 분리(SoC: Separation of Concerns)**와 맥을 같이합니다.
- MVC의 컨트롤러가 처리해야 하는 핵심 로직(비즈니스 로직)과 공통적으로 적용되어야 하는 “예외 처리” 등의 로직을 별도의 클래스(Advice)로 분리한다는 점이 AOP의 아이디어와 유사합니다.

**스프링 내부 동작 방식**

- 스프링은 @ControllerAdvice(혹은 @RestControllerAdvice)로 등록된 클래스를 빈(Bean)으로 등록하고, DispatcherServlet 단계에서 발생하는 예외를 가로채어 해당 Advice가 처리하도록 합니다.
- **일반적인 AOP(예: @Aspect, spring aop)에서 메서드 호출 이전/이후에 프록시(proxy, 가짜 객체)를 두어 로직을 삽입하는 것**과 달리,
- @RestControllerAdvice는 주로 **예외 발생 시** 스프링 컨테이너가 특정 핸들러(Advice 메서드)를 찾아서 실행한다는 점에서, **“예외 처리”에 특화된 AOP적인 구조**를 갖춘다고 볼 수 있습니다.

**핵심 차이점**

- @Aspect 를 이용한 AOP는 스프링이 **프록시 객체**를 만들어 메서드 앞뒤로 공통 로직을 삽입하는 방식을 주로 사용합니다. 
  - (메소드 실행 전/후, 예외 발생 시점 등)
- @RestControllerAdvice는 이러한 **프록시 방식**보다는 **스프링 MVC 내부의 HandlerExceptionResolver** 기능과 결합하여, **컨트롤러 전역적인 예외나 응답 처리를 한 곳에서 담당**하도록 구현된 것입니다.
- **따라서 @RestControllerAdvice는 “AOP 사상”을 예외 처리에 적용하기 위해 스프링이 제공하는 편의 기능**이라고 이해하면 됩니다.



> [!TIP]
>
> ### @ControllerAdvice란?
>
> - `@ExceptionHandler`, `@ModelAttribute`, `@InitBinder` 가 적용된 메서드들에 `AOP`를 적용해 `Controller` 단에 적용하기 위해 고안된 어노테이션 입니다.
> - 클래스에 선언하면 되고 모든 @Controller에 대해 전역적으로 발생할 수 있는 예외를 잡아서 처리할 수 있습니다.
>
> ---
>
> ### @ResponseBody
>
> - **반환 값을 HTTP 응답 본문으로 변환**:
>
>   - 컨트롤러 메서드가 반환하는 객체(예: DTO, 엔티티 등)를 HTTP 응답 본문으로 변환합니다.
>   - 예를 들어, 객체를 JSON 형식으로 변환하여 클라이언트에게 전달할 수 있습니다.
>
>   **뷰(View)를 사용하지 않음**:
>
>   - **기존의 Spring MVC에서는 컨트롤러 메서드가 반환하는 값이 뷰 이름(view name)으로 해석되어 JSP, Thymeleaf 등의 뷰 템플릿을 통해 HTML을 생성했습니다.**
>   - `@ResponseBody`를 사용하면 뷰를 거치지 않고, 직접 데이터를 응답 본문에 작성합니다.
>
>   **HTTP 메시지 컨버터(Message Converter) 사용**:
>
>   - Spring은 `@ResponseBody`가 붙은 메서드의 반환 값을 적절한 형식(JSON, XML 등)으로 변환하기 위해 **HTTP 메시지 컨버터**를 사용합니다.
>
>   - 예를 들어, Jackson 라이브러리가 클래스패스에 있으면 객체를 JSON으로 변환합니다.
>
>     
>
> ``` java
> @RestController
> public class UserController {
> 
>     @GetMapping("/user/{id}")
>     @ResponseBody // 생략 가능 (RestController에서 자동 적용)
>     public User getUser(@PathVariable Long id) {
>         User user = new User(id, "John Doe", "john.doe@example.com");
>         return user; // User 객체가 JSON으로 변환되어 응답 본문에 작성됨
>     }
> }
> ```
>
> ### @ResponseBody와 @RestController의 관계
>
> - `@RestController`는 `@Controller`와 `@ResponseBody`를 합친 어노테이션입니다.
> - `@RestController`가 붙은 클래스의 모든 메서드는 `@ResponseBody`가 적용된 것처럼 동작합니다.
> - 따라서 RESTful 웹 서비스를 개발할 때는 `@RestController`를 사용하는 것이 일반적입니다.





