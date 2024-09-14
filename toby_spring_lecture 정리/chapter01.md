# 강사님이 사용하시는 개발방법
 
- 빠르게 완성해서 간단한 방법을 찾는다.<br>
- 작성한 코드가 동작하는지 확인하는 방법을 찾는다.<br>
- 조금씩 기능을 추가하고 다시 검증한다.<br>
  - 한 스탭에 할 수있는 정도 범위로 나눠서 계속 반복해나가기
- 코드를 한눈에 이해하기 힘들다면 코멘트로 설명을 달아준다.<br>


##

# chaper01.

Payment 클래스

java spring 에서 오브젝트 생성 시 포인트,

- 포인트
    - 필드의 값을 변경하는 주체는  어떤 메서드, 혹은 생성자를 통해서만 일어나도록 만들기 위해 private 사용을 권장한다.
    - 대부분 해당 오브젝트(객체)가 생성되는 시점에 데이터를 생성하기에
        - 생성자를 이용한 객체 생성이 좋다.
        - 생성자를 이용한 객체생성시 파라미터의 타입이 동일하다면 실수해서 값을 다르게 입력할수 잇다.
        - 문제 예시) 외화금액을 넣어야 할 자리에 원화금액을 넣는다던가...
        
    - 연산에서 정확한 계산을 위한 숫자를 다룰때는 반드시 bigdecimal을 사용해야한다.
        - Long , Double은 오차가 발생할 위험이 있다
        - 돈에 대한 연산에서 오차가 발생하면 대형사고
    
    - 일반적으로 Getter는 대부분의 오브젝트에서 사용한다.
    - 클래스의 필드값을 알고 싶을때는?
        - toString()을 사용한다.
    
    - 추가적인 의견
        - 인텔리제이의 단축키를 적극적으로 활용하는 것을 추천함
    

```java

public class Payment {
    // 필드의 값을 변경하는 것은 어떤 메서드, 혹은 생성자를 통해서만 일어나도록 만들기 위해 private 사용
    // 일반적으로 아이디 등을 지정할때 사용하는 숫자타입은 앵간하면 long
    private Long orderId;
    private String currency; // 화페종류
    // 정확한 계산을위한 숫자를 다룰때는 반드시 bigdecimal을 사용해야한다.
    private BigDecimal foreignCurrencyAmount; // 외화 금액
    private BigDecimal exRate; // 적용환율
    private BigDecimal convertedAmount; // 원화 금액
    private LocalDateTime vaildUntil; //유효시간

    // 대부분 해당 오브젝트(객체)가 생성되는 시점에 데이터를 생성해야하는 경우가 대부분이기에
    // 생성자를 이용한 객체 생성이 좋다.
    // 생성자를 이용한 객체생성시 파라미터의 타입이 동일하다면 실수해서 값을 다르게 입력할수 잇다..
    // ex) 외화금액을 넣어야 할 자리에 원화금액을 넣는다던가...
    public Payment(Long orderId, String currency,
            BigDecimal foreignCurrencyAmount,
            BigDecimal exRate,
            BigDecimal convertedAmount,
            LocalDateTime vaildUntil) {
        this.orderId = orderId;
        this.currency = currency;
        this.foreignCurrencyAmount = foreignCurrencyAmount;
        this.exRate = exRate;
        this.convertedAmount = convertedAmount;
        this.vaildUntil = vaildUntil;
    }

    // Getter는 일반적으로 자주 사용한다.
    public Long getOrderId() {
        return orderId;
    }

    public String getCurrency() {
        return currency;
    }

    public BigDecimal getForeignCurrencyAmount() {
        return foreignCurrencyAmount;
    }

    public BigDecimal getExRate() {
        return exRate;
    }

    public BigDecimal getConvertedAmount() {
        return convertedAmount;
    }

    public LocalDateTime getVaildUntil() {
        return vaildUntil;
    }

    // 필드 정보를 출력하기 위한 toString()
    @Override
    public String toString() {
        return "Payment{" +
                "orderId=" + orderId +
                ", currency='" + currency + '\'' +
                ", foreignCurrencyAmount=" + foreignCurrencyAmount +
                ", exRate=" + exRate +
                ", convertedAmount=" + convertedAmount +
                ", vaildUntil=" + vaildUntil +
                '}';
    }
}

```

