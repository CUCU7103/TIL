Payment의 변경없이 환율정보를 가져오는 방법을 확장하게 만드려면

1. 환율을 가져오는 방법
2. Payment를 세팅

위의 두 가지가 클래스 레벨에서 분리되어지면 된다.

```java
public class PaymentService {

    ...생략

    // 환율 정보를 가져오는 메서드
    // 지정된 통화 코드에 맞는 환율 정보를 외부 API에서 가져옴
    private BigDecimal getExRate(String currency) throws IOException {
        // 외부 API의 URL을 생성 (예: USD 환율 정보를 가져옴)
        URL url = new URL("https://open.er-api.com/v6/latest/" + currency);
        // API와의 연결을 열고 데이터 읽기
        HttpURLConnection connection = (HttpURLConnection) url.openConnection(); // 예외처리가 필요함.
        BufferedReader br = new BufferedReader(new InputStreamReader(connection.getInputStream()));
        // 응답 데이터를 라인으로 읽어들여서 하나의 문자열로 합침
        String response = br.lines().collect(Collectors.joining());
        br.close();

        // ObjectMapper를 이용해 JSON 응답을 Java 객체로 변환
        ObjectMapper mapper = new ObjectMapper();
        ExRateData data = mapper.readValue(response, ExRateData.class);
        // 변환된 객체에서 KRW(원화)에 대한 환율 값을 추출
        BigDecimal exRate = data.rates().get("KRW");
        // 환율 반환
        return exRate;
    }

    // main 메서드: 코드를 실행하는 진입점
    public static void main(String[] args) throws IOException {
        // PaymentService 객체를 생성
        PaymentService paymentService = new PaymentService();
        // USD 통화의 50.7 단위 외화를 원화로 변환하고 결제 정보 생성
        Payment payment = paymentService.prepare(100L, "USD", BigDecimal.valueOf(50.7));
        // 결제 정보 출력
        System.out.println(payment);
    }
}

```

위와 같았던 기존의 코드를 아래와 같이 변경하였다.

PaymentService를 추상클래스로 변경하고,상속을 사용하여 getExRate 메서드를  별도의 클래스로 분리하였다.

```java
팩토리 메소드 패턴
팩토리 메소드 패턴은 상속을 통해 기능을 확장하게 하는 패턴이다. 
슈퍼클래스 코드에서는 서브클래스에서 구현할 메소드를 호출해서 필요한 타입의 오브젝트를 가져와 사용한다. 이 메소드는 주로 인터페이스 타입으로 오브젝
트를 리턴하므로 서브클래스에서 정확히 어떤 클래스의 오브젝트를 만들어 리턴할지는 슈퍼클래스에서는 알지 못
한다. 
사실 관심도 없다. 
서브클래스는 다양한 방법으로 오브젝트를 생성하는 메소드를 재정의할 수 있다. 
이렇게 서브클래스에서 오브젝트 생성 방법과 클래스를 결정할 수 있도록 미리 정의해둔 메소드를 팩토리 메소드라고 하고, 
이 방식을 통해 오브젝트 생성 방법을 나머지 로직, 즉 슈퍼클래스의 기본 코드에서 독립시키는 방법을 팩토리 메소드 패턴이라고 한다.
```

```java
// 추상 메서드를 사용하기 위해 추상클래스로 변경
abstract public class PaymentService {
    // 결제 준비 메서드: 주문 ID, 통화 코드, 외화 금액을 받아 처리
    public  Payment prepare(Long orderId, String currency, BigDecimal foreignCurrencyAmount)
            throws IOException {
        // 지정한 통화 코드로 환율 정보를 가져옴
        BigDecimal exRate = getExRate(currency); // 원화 환율
        // 환율을 사용해 외화 금액을 원화 금액으로 변환
        BigDecimal convertedAmount = foreignCurrencyAmount.multiply(exRate); // 환율을 곱하여 원화 금액으로 변환
        // 결제 유효 시간을 현재 시간에서 30분 후로 설정
        LocalDateTime validUntil = LocalDateTime.now().plusMinutes(30); // 유효시간
        // 결제 정보를 담은 Payment 객체 생성 후 반환
        return new Payment(orderId, currency, foreignCurrencyAmount, exRate, convertedAmount, validUntil);
    }

    // 추상 메서드로 변환
    // 팩토리 메소드 패턴
    abstract BigDecimal getExRate(String currency) throws IOException ;

}
```

```java

// PaymentService를 상속받은 WebApiExRatePaymentService  클래스
public class WebApiExRatePaymentService extends PaymentService {

    @Override
    BigDecimal getExRate(String currency) throws IOException {
           // 외부 API의 URL을 생성 (예: USD 환율 정보를 가져옴)
            URL url = new URL("https://open.er-api.com/v6/latest/" + currency);
            // API와의 연결을 열고 데이터 읽기
            HttpURLConnection connection = (HttpURLConnection) url.openConnection(); // 예외처리가 필요함.
            BufferedReader br = new BufferedReader(new InputStreamReader(connection.getInputStream()));
            // 응답 데이터를 라인으로 읽어들여서 하나의 문자열로 합침
            String response = br.lines().collect(Collectors.joining());
            br.close();

            // ObjectMapper를 이용해 JSON 응답을 Java 객체로 변환
            ObjectMapper mapper = new ObjectMapper();
            ExRateData data = mapper.readValue(response, ExRateData.class);
            // 변환된 객체에서 KRW(원화)에 대한 환율 값을 추출
            BigDecimal exRate = data.rates().get("KRW");
            // 환율 반환
            return exRate;
    }
}
```

```java
// PayMentService를 상속받은 SimpleExRatePaymentService 클래스
//
public class SimpleExRatePaymentService extends PaymentService{

    @Override
    BigDecimal getExRate(String currency) throws IOException {
        if(currency.equals("USD")) return BigDecimal.valueOf(1000);
        throw new IllegalArgumentException("지원되지 않는 통화입니다.");
    }
}
```

또한 main메서드를 이동시켯다

```java
public class Client {
    // main 메서드: 코드를 실행하는 진입점
    public static void main(String[] args) throws IOException {
               
         // 아래의 부분만 수정하면 상황에 맞는 환율정보를 담은 클래스를 적용할 수있다.
        PaymentService paymentService = new SimpleExRatePaymentService();\
        Payment payment = paymentService.prepare(100L, "USD", BigDecimal.valueOf(50.7));
        System.out.println(payment);
    }
}
```

기존 코드의 변화

```java
abstract public class PaymentService {
    // 결제 준비 메서드: 주문 ID, 통화 코드, 외화 금액을 받아 처리
    public  Payment prepare(Long orderId, String currency, BigDecimal foreignCurrencyAmount)
            throws IOException {
        // 지정한 통화 코드로 환율 정보를 가져옴
        BigDecimal exRate = getExRate(currency); // 원화 환율
        // 환율을 사용해 외화 금액을 원화 금액으로 변환
        BigDecimal convertedAmount = foreignCurrencyAmount.multiply(exRate); // 환율을 곱하여 원화 금액으로 변환
        // 결제 유효 시간을 현재 시간에서 30분 후로 설정
        LocalDateTime validUntil = LocalDateTime.now().plusMinutes(30); // 유효시간
        // 결제 정보를 담은 Payment 객체 생성 후 반환
        return new Payment(orderId, currency, foreignCurrencyAmount, exRate, convertedAmount, validUntil);
    }

    // 환율 정보를 가져오는 메서드
    // 지정된 통화 코드에 맞는 환율 정보를 외부 API에서 가져옴
    // 클래스 단위로 코드를 분리하였다.
    abstract BigDecimal getExRate(String currency) throws IOException ;

}
```

기존 코드에 비해서 훨씬 코드가 깔끔해진 것을 확인 할 수 있다.

단 상속을 사용하는 것에 대해서는 단점이 잇는데

강의에서는 아래와 같이 설명했다

```java
자바는 다중 상속을 허용하지 않는다 따라서 또 다른 관심사를 분리할 경우 
확장을 이용하기가 매우 어렵다. 
또, 상속은 상하위 클래스의 관계매 매우 밀접하다. 
상위 클래스의 변경에 따라 하위 클래스를 모두 변경해야 하는 상황이 생길 수도 있다. 
디자인 패턴에 일부 적용된 특별한 목적을 위해서 사용되는 경우가 아니라면
상속을 통한 확장은 관심사의 분리에 사용하기엔 분명한 한계가 있다.
```
