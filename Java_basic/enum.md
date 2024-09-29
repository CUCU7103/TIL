# enum

- final로 기본자료형의 값을 고정할 수 있고 이렇게 고정한 값을 상수라고 한다.
- 이러한 상수들로만 구성된 클래스를 enum 이라고한다.

## enum 클래스의 사용방법

```
public enum OvertimeValue {
    THREE_HOUR,
    FIVE_HOUR,
    WEEKEND_FOUR_HOUR,
    WEEKEND_EIGHT_HOUR;
}


public class OvertimeManager {

    public int getOverTimeAmount(OvertimeValue value) {
        int amount = 0;
        System.out.println(value);
        switch (value) {
            case THREE_HOUR:
                amount = 18000;
                break;
            case FIVE_HOUR:
                amount = 30000;
                break;
            case WEEKEND_FOUR_HOUR:
                amount = 40000;
                break;
            case WEEKEND_EIGHT_HOUR:
                amount = 60000;
                break;
        }
        return amount;
    }
}

================================================
public class Main1 {

    public static void main(String[] args) {
        OvertimeManager manager = new OvertimeManager();
        int myAmount =manager.getOverTimeAmount(OvertimeValue.THREE_HOUR);
        System.out.println(myAmount);
    }
}

```
- enum은 switch문에 사용하기 적합하다.
- enum의 객체생성은 "enum클래스명.상수 이름"


또한 상수의 값을 지정하여 사용이 가능하다.













