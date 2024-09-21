# Java8에서의 주요한 변경사항

1. 람다 표현식

람다 표현식의 구조

```java
(매개변수) -> { 실행할 코드 }

(int x, int y) -> {
    return x + y;
}

매개변수가 하나일 때는 괄호를 생략할 수 있습니다.

x -> x * x

실행할 코드가 한 줄이고 반환값이 있는 경우 중괄호와 return 키워드를 생략할 수 있습니다.

(a, b) -> a + b

```

**람다 표현식을 사용하는 이유:**

- **간결성:** 코드를 더 짧고 읽기 쉽게 만들어 줍니다.
- **익명 함수 지원:** 별도의 메서드를 정의하지 않고도 함수를 인수로 전달할 수 있습니다.
- **함수형 프로그래밍 지원:** 자바에서 함수형 프로그래밍 패러다임을 도입하여 컬렉션 처리 등을 더욱 효율적으로 할 수 있습니다.

**람다 표현식을 사용할 수 있는 조건:**

- 람다 표현식은 **함수형 인터페이스**에서만 사용할 수 있습니다.
- 함수형 인터페이스란 하나의 추상 메서드만 가지는 인터페이스를 말하며,
    - `@FunctionalInterface` 애노테이션을 사용하여 표시할 수 있습니다

```java

// 함수형 인터페이스

@FunctionalInterface
interface MathOperation {
    int operation(int a, int b);
}

public class CustomFunctionInterfaceExample{

		public static void main(String[] args) {
			
			// 덧셈을 수행하는 람다 표현식
			MathOperation addtion = (a,b) -> a + b;
			
			// 뺄셈을 수행하는 람다 표현식
			MathOperation substraction = (a,b) -> a-b;	
		 
		 System.out.println("10 + 5 = " + addition.operation(10, 5)); // 출력: 10 + 5 = 15
     System.out.println("10 - 5 = " + subtraction.operation(10, 5)); // 출력: 10 - 5 = 5
		
		}

}

```

1. 스트림 (stream)
- 람다를 활용해 배열과 컬렉션을 함수형으로 간단하게 처리할 수 있는 기술

- 특징
    - 원본 데이터 소스를 변경하지 않는다.
    - 일회용이다.
    - 최종 연산 전까지 중간연산을 수행하지 않는다.
    - 작업을 내부 반복으로 처리한다
    - 병렬 처리가 쉽다.

- 예시

```java
import java.util.Arrays;
import java.util.List;

public class StreamExample {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David", "Eve");

        // 'C'로 시작하는 이름을 필터링하고 대문자로 변환한 후 출력
        names.stream()
            .filter(name -> name.startsWith("C")) // C라는 이름 필터링
            .map(String::toUpperCase) // 대문자로 변경
            .forEach(System.out::println); // 출력한다.
    }
}
/*
출력:
CHARLIE
*/

```

1. 인터페이스의 Default 메서드

```java
public interface Calculator {
    int add(int a, int b);

    // 디폴트 메소드
    default int multiply(int a, int b) {
        return a * b;
    }
}

public class SimpleCalculator implements Calculator {
    @Override
    public int add(int a, int b) {
        return a + b;
    }
    
    // 추가된 디폴트 메서드를 구현하지 않아도 된다.
}

public class CalculatorTest {
    public static void main(String[] args) {
        Calculator calculator = new SimpleCalculator();
        System.out.println("더하기: " + calculator.add(5, 3));       // 출력: 더하기: 8
        System.out.println("곱하기: " + calculator.multiply(5, 3)); // 출력: 곱하기: 15
    }
}

```

기존 인터페이스에는 추상 메서드만 사용이 가능햇으나 default 메서드를 추가적으로 사용할 수 있게 되었다.

왜 사용할까?

- 인터페이스를 구현하는 클래스에서는 메서드를 모두 구현해야하기 때문에 인터페이스에 메서드를 추가할때 문제가 발생한다
- 메서드 하나를 추가하려면 해당 인터페이스를 구현하는 모든 클래스에서는 해당 메서드를 모두 구현해줘야 하는 것

이러한 문제를 default 메서드를 사용하므로써 인터페이스에 새로운 메소드를 추가하면서도 기존의 구현 클래스에 영향을 주지 않고 **하위 호환성을 유지**할 수 있습니다.

4. 새로운 날짜와 시간 API (New Date and Time API)

| LocalDate | 날짜를 표현하는 클래스로, 연, 월, 일 정보만 포함 |
| --- | --- |
| LocalTime | 시간을 표현하는 클래스로, 시, 분, 초, 밀리초 정보만 포함 |
| LocalDateTime | 날짜와 시간을 모두 포함하는 클래스로, LocalDate와 LocalTime의 조합 |
| Instant | 기계적인 시간의 흐름을 나타내는 클래스로, 에포크부터 경과한 시간 표현 |
| Duration | 두 시간 간의 지속 시간을 나타내는 클래스로, 시간 기반의 간격 표현 |
| Period | 두 날짜 간의 지속 기간을 나타내는 클래스로, 날짜 기반의 간격 표현 |
| DateTimeFormatter | 날짜와 시간을 포맷팅하고 파싱하기 위한 클래스 |

예시

```java
import java.time.LocalDate;
import java.time.LocalDateTime;
import java.time.LocalTime;
import java.time.format.DateTimeFormatter;

public class DateTimeExample {
    public static void main(String[] args) {
        // 현재 날짜와 시간
        LocalDateTime currentDateTime = LocalDateTime.now();
        System.out.println("현재 날짜와 시간: " + currentDateTime);

        // 특정 날짜 설정
        LocalDate date = LocalDate.of(2024, 9, 19);
        System.out.println("설정된 날짜: " + date);

        // 특정 시간 설정
        LocalTime time = LocalTime.of(14, 30, 0);
        System.out.println("설정된 시간: " + time);

        // 날짜 포맷팅
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
        String formattedDateTime = currentDateTime.format(formatter);
        System.out.println("포맷된 날짜와 시간: " + formattedDateTime);
    }
}
/*
출력 예시:
현재 날짜와 시간: 2024-09-19T12:45:30.123
설정된 날짜: 2024-09-19
설정된 시간: 14:30
포맷된 날짜와 시간: 2024-09-19 12:45:30
*/

```

**왜 자바 8에서는 java.time 패키지가 새로 추가되었을까?**

기존 `java.util.Date`와 `java.util.Calendar` 클래스를 사용하는데 주요한 문제점이 있엇다.

1. 가변성
    1. 가변객체로 설계되어져서 다중 스레드 환경에서 동시성 문제가 발생할 가능성이 컷다.

 2. **잘못된 월 인덱스**

- `java.util.Calendar` 클래스는 월을 0부터 시작하는 인덱스 방식으로 관리했습니다. 예를 들어, 1월은 `0`, 12월은 `11`로 표현되었기 때문에, 이를 사용하는 프로그래머들이 종종 실수를 하곤 했습니다.
    - 반면, `java.time` 패키지에서는 월이 `1`부터 시작하여 이러한 혼란을 방지할 수있다.
    
1. **시간대(Timezone) 처리의 어려움**
- `java.util.Date`는 기본적으로 시간대(TimeZone)를 고려하지 않고, UTC(협정 세계시)를 기반으로 처리하는 경우가 많았습니다.
- 시간대 처리를 하기 위해서는 추가적인 작업이 필요했으며, 이를 다루는 것이 매우 복잡했습니다.

java.time 패키지는 이전 API들보다 훨씬 더 직관적이고 불변(immutable) 객체를 사용하여 쓰레드 세이프(thread-safe)한 처리를 제공합니다.

1. 옵셔널 클래스 (Optional Class)

```java
import java.util.Optional;

public class OptionalExample {
    public static void main(String[] args) {
        String[] words = new String[5];
        words[2] = "Optional 클래스 예제";

        // 기존 방식: Null 체크 필요
        if (words[2] != null) {
            System.out.println("단어 길이: " + words[2].length());
        } else {
            System.out.println("값이 없습니다.");
        }

        // Optional 사용 방식
        // 명시적으로 null 값을 처리할 수 있다.
        Optional<String> optionalWord = Optional.ofNullable(words[2]);
        optionalWord.ifPresent(word -> System.out.println("단어 길이: " + word.length()));
        System.out.println("값이 존재합니까? " + optionalWord.isPresent());
    }
}
/*
출력:
단어 길이: 12
값이 존재합니까? true
*/

====================

// 기존 방식: null 체크 필요
String name = getName();
if (name != null) {
    System.out.println(name.length());
}

// Optional 사용
Optional<String> nameOptional = getNameOptional();
nameOptional.ifPresent(n -> System.out.println(n.length()));
```

**Optional 클래스**는 **null 값을 안전하게 처리하기 위한 도구**입니다. 

프로그래밍을 할 때 변수나 객체가 null일 수 있는데, 이것을 잘못 다루면 **NullPointerException**이 발생합니다.

**Optional**은 값이 있을 수도 있고 없을 수도 있는 상황을 명시적으로 표현합니다. 

**즉, 어떤 메서드가 값을 반환할 때 그 값이 존재하지 않을 수 있음을 나타내기 위해 Optional을 사용합니다.**

**Optional의 주요 메서드:**

- `of(T value)`: null이 아닌 값으로 Optional 생성
- `ofNullable(T value)`: 값이 null일 수 있는 Optional 생성
- `isPresent()`: 값이 존재하면 true
- `ifPresent(Consumer<? super T> action)`: 값이 있을 때 동작 수행
- `orElse(T other)`: 값이 없을 때 다른 값 반환
- `orElseGet(Supplier<? extends T> supplier)`: 값이 없을 때 동적 값 반환
- `orElseThrow(Supplier<? extends X> exceptionSupplier)`: 값이 없을 때 예외 발생

**왜 Optional을 사용할까요?**

- **NullPointerException 방지**: null 체크를 깜빡해서 발생하는 오류를 줄입니다.
- **코드 가독성 향상**: 값이 없을 수 있음을 명시적으로 나타내어 코드 이해를 돕습니다.
- **함수형 프로그래밍 지원**: map, filter 같은 메서드를 활용하여 코드를 간결하게 작성합니다.
