# java.util.Date vs java.time.LocalDate



자바의 Date 클래스 

- 날짜와 시간을 모두 포함하였습니다.
- **내부적으로 밀리초를 기반으로 동작합니다**.
- 1900년을 기준으로 하는 오프셋, 0에서 시작하는 달 인덱스 등 모호한 설계로 유용성이 떨어집니다.
- **불변객체가 아니기에 사이드 이펙트가 발생할 가능성이 매우 높습니다.**.
- **형식과 의미가 명확하지 않아서 사용하기가 불편하고 직관적이지 않습니다.**

> [!NOTE]
>
> 불변 객체란?
>
> - 객체의 상태(객체 내부의 값, 필드, 멤버 변수)가 변하지 않는 객체를 불변 객체(Immutable Object)라 한다.

주요 메서드

- `getTime()`: 타임스탬프를 반환.
- `toString()`: 읽기 어려운 기본 형식으로 출력.

```  java
import java.util.Date;

public class Main {
    public static void main(String[] args) {
        Date date = new Date(); // 현재 날짜와 시간
        System.out.println("Date: " + date);

        date.setTime(date.getTime() + 86400000L); // 하루 추가
        System.out.println("Modified Date: " + date);
    }
}
```





자바의 LocalDate 클래스 

- java.time 패키지의 일부입니다.
- 날짜(연도, 월, 일)만 표현하며 시간정보는 표현하지 않습니다.
- 불변객체로 설계되어 Thread safe 합니다.
- 명확하고 직관적인 api를 제공합니다.
- 타임존이나 시간 정보를 포함하지 않아 순수 날짜 작업에 적합.



**주요 메서드:**

- `now()`: 현재 날짜 반환.
- `of(year, month, day)`: 특정 날짜 생성.
- `plusDays(days)`, `minusMonths(months)`: 날짜 연산.
- `format(DateTimeFormatter)`: 지정된 포맷으로 출력.



``` java
import java.time.LocalDate;

public class Main {
    public static void main(String[] args) {
        LocalDate date = LocalDate.now(); // 현재 날짜
        System.out.println("LocalDate: " + date);

        LocalDate nextDay = date.plusDays(1); // 하루 추가
        System.out.println("Next Day: " + nextDay);
    }
}
```






