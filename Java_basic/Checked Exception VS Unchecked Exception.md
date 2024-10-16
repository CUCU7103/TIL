# Checked Exception VS Unchecked Exception



> [!TIP]
>
> #### 예외처리를 하는 이유
>
> - 프로그램의 비정상적인 종료를 막고 상태를 정상상태로 유지하기 위함입니다
>
> #### 예외를 마지막까지 던진다면?
>
> - 모든 곳에서 발생한 예외를 잡지 않았기 때문에 결과적으로 main() 밖으로 예외가 던져집니다.
>
> - 밖으로 예외가 던져지면 예외 메시지와 예외를 추적할 수 있는 스택 트레이스를 출력하고 프로그램을 종료되어집니다.
>
> #### 처리할 수 없는 예외가 발생하였다면?
>
> - 사용자에게 적절한 오류메시지를 제공해주는 것이 좋다.

![373105399-9ab12434-dabc-48a3-8238-f0496f8aa821.png (695×444)](https://private-user-images.githubusercontent.com/107477191/373105399-9ab12434-dabc-48a3-8238-f0496f8aa821.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjkwODE3OTcsIm5iZiI6MTcyOTA4MTQ5NywicGF0aCI6Ii8xMDc0NzcxOTEvMzczMTA1Mzk5LTlhYjEyNDM0LWRhYmMtNDhhMy04MjM4LWYwNDk2ZjhhYTgyMS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQxMDE2JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MTAxNlQxMjI0NTdaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1lOWJjMDc1MjExZGE1ZTdiYmE3ZGFmMWYxYjFlMjViMjI5YmNkN2VmZjllY2Q1ZTUyZmUyNjc5ZTJjNTdkOTFiJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.idGSnnbhDhC4dodUTsWB7TelQCJtnAPBHhPAUeYnN7o)



### Checked Exception 

- 자바 컴파일러가 컴파일 시점에 체크하는 예외입니다.
- 체크 예외는 잡아서 직접 처리(try-catch문)하거나 또는 밖(throws)으로 던지거나 둘중 하나를 개발자가 직접 명시적으로 처리해야합니다. 그렇지 않으면 컴파일 오류가 발생합니다. 
- 컴파일러는 문법적인 오류와 체크 예외(Checked Exception) 처리 여부 등을 검사합니다.
- 컴파일이 성공하면 바이트코드가 생성되며, 이 바이트코드는 이후 런타임 시점에서 JVM에 의해 실행됩니다
- Exception을 상속받아 만들어집니다.

#### 장점

- 컴파일 시점에 확인되어지기에 예외처리를 강제하여 프로그램의 안정성을 높일 수 있습니다.

#### 단점

- 반드시 예외처리를 해야합니다.

- 코드의 가독성과 복잡성이 크게 증가합니다.

  - 다양한 메서드들이 서로 호출되어질때 발생하는 모든 예외를 처리하거나 전달해야 하므로 복잡해집니다.
  - 체크 예외의 부담: 처리할 수 없는 예외는 밖으로 던져야 한다. 체크 예외이므로 throws 에 던질 대상을 일일이 명시해야 한다

- 처리할 수없는 예외에 대한 강제처리 진행

  



#### Unchecked Exception 

- 컴파일러에서 체크하지 않는 예외입니다. 
- 언체크 예외는 예외를 try-catch문으로 잡거나 던지지 않아도 됩니다.
- 예외를 잡지 않으면 throws를 사용하지 않아도 자동으로 호출한 곳으로 예외를 던집니다.
- RuntimeException을 상속받아 만들어집니다.

#### 장점

- 컴파일러가 예외 처리를 강제하지 않기 때문에, 개발자가 try-catch 블록을 남용하지 않아도 됩니다. 이를 통해 코드가 더 깔끔하고 간결해집니다.
- 불필요한 상황에서 예외처리 코드를 작성하지 않아도 됩니다.

- **코드의 상위 레벨에서 한 번에 예외를 처리할 수 있습니다. 이로 인해 각 메서드마다 예외를 처리할 필요 없이, 전체적으로 중요한 곳에서 한 번에 예외를 처리할 수 있습니다.**
- 주로 개발자의 실수나 잘못된 로직으로 발생하기에 빠르게 오류를 탐지하고 수정할 수있게 해줍니다.

#### 단점

- 예외처리를 강제하지 않아 중요한 예외가 발생했음에도 불구하고 이를 무시하거나, 예외 발생 사실을 인지하지 못해 시스템 장애로 이어질 수 있습니다



### 사용하는 시점

**Checked Exception 사용 시기**:

- 외부 시스템과 상호작용하는 경우(파일, 데이터베이스, 네트워크 등).
- 예외가 발생했을 때 **복구할 가능성**이 있는 경우.
- 반드시 예외를 처리해야 하는 중요한 작업일 때.

#### **실무에서의 적용 전략**

- **필수적 예외 처리**:
  -  Checked Exception은 **반드시 처리해야 하는** 예외를 의미하므로, 외부 시스템과의 상호작용에서 발생하는 예외는 try-catch 블록으로 처리하거나 메서드에 `throws`로 선언하여 호출자가 처리하게 만듭니다.
- **예외 복구**: 
  - **예외가 발생할 때, 상황에 맞게 재시도하거나 기본 값을 제공하는 방식으로 복구를 시도할 수 있습니다.**
- **비즈니스 로직과 분리**: 
  - Checked Exception은 **주로 외부 시스템과의 상호작용에서 발생**하므로, 비즈니스 로직과 분리하여 에러 처리 전용 클래스를 만들거나, 외부 자원 처리 전용 메서드로 분리하는 것이 좋습니다.



**Unchecked Exception 사용 시기**:

- **프로그래머의 실수**나 **논리적 오류**를 나타낼 때.
- 발생 시 복구가 불가능한 경우(예: `NullPointerException`, `IllegalArgumentException`).
- 비즈니스 로직에서 잘못된 입력이나 상태를 처리할 때, 이를 미리 검증하고 예외를 던질 때.

#### **실무에서의 적용 전략**

- **비즈니스 로직에서의 검증**: 

  - Unchecked Exception은 주로 프로그래머의 실수나 잘못된 로직에서 발생하기 때문에, 비즈니스 로직에서 필요한 검증을 미리 수행하는 것이 중요합니다. 
    - 예를 들어, 메서드 호출 전에 인자의 범위를 검증하거나 객체가 null인지 확인하는 방식입니다.

- **상위 레벨에서 예외 처리**: 

  - Unchecked Exception은 **개별 메서드에서 처리하지 않고 상위 레벨의 예외 처리 핸들러에서 한꺼번에 처리할 수 있습니다.** 
    - 예를 들어, Spring에서는 `@ControllerAdvice`를 사용해 모든 Unchecked Exception을 한 곳에서 처리할 수 있습니다.

  ```JAVA
  @ControllerAdvice
  public class GlobalExceptionHandler {
      @ExceptionHandler(NullPointerException.class)
      public ResponseEntity<String> handleNullPointerException(NullPointerException ex) {
          return new ResponseEntity<>("Null pointer exception occurred.", HttpStatus.INTERNAL_SERVER_ERROR);
      }
  }
  ```

- **의미 있는 예외 던지기**:

  -  Unchecked Exception은 프로그램의 논리적 오류를 나타낼 때 사용되므로, 적절한 예외 메시지를 포함해 개발자가 문제의 원인을 쉽게 파악할 수 있도록 해야 합니다. 
    - 의미 없는 예외 메시지를 던지면 디버깅이 어렵습니다.



## 현대 개발에서는 어떤 예외처리를 더 많이 사용할까?

- ### **Unchecked Exception을 더 많이 사용합니다**.

이유

- 실무에서는 생각보다 내가 처리할 수있는 예외가 많지 않습니다.
  - 네트워크 서버에 문제가 발생하여 통신이 안되는 경우
  - 데이터베이스 서버에 문제 발생하여 접속이 안되는 경우 등등
  - 수많은 라이브러리를 사용하고 , 다양한 외부시스템과 연동되어집니다.

- 체크 예외에 대한 부담.
  - 개발자가 실수로 놓칠 수 잇는 예외를 잡아주는 점에서 좋으나 **처리할 수 없는 예외가 많아지고** 또 **프로그램이 복잡해지면서 체크 예외를 사용하는 것이 점점 더 부담스러워졌습니다**.
    - 체크 예외는 반드시 try-catch로 잡아주거나 throws로 던져야 하기 때문입니다.



