# 관심사의 분리

관심사는 변경이라는 관점으로 이야기 할 수 있다.

ex) 계산하는 방식이 조금 달라진다거나 , 시간이 변경된다거나

```java
        // 환율정보를 구하는 로직
        URL url = new URL("https://open.er-api.com/v6/latest/" + currency);
        HttpURLConnection connection = (HttpURLConnection) url.openConnection(); // 예외처리가 필요함.
        BufferedReader br = new BufferedReader(new InputStreamReader(connection.getInputStream()));
        String response =  br.lines().collect(Collectors.joining());
        br.close();
        
       
        ObjectMapper mapper = new ObjectMapper();
        ExRateData data = mapper.readValue(response,ExRateData.class);
        BigDecimal exRate = data.rates().get("KRW");

=========================================================================================

        // 환전한 금액을 구하는 로직
        BigDecimal convertedAmount = foreignCurrencyAmount.multiply(exRate);
        // 유효시간을 지정하는 로직
        LocalDateTime validUntil = LocalDateTime.now().plusMinutes(30);


```

위의 로직들은 서로의 관심사가 다른데 같은 메서드 안에 있다.
즉 분리가 필요함. 
하나의 메서드 안에 있는 코드블럭을 다른 메서드로 꺼내는 것을 메소드 추출이라고 한다

메소드 추출을 통한 코드 리팩터링

```java
public class PaymentService {
    
    // 결제 준비 메서드: 주문 ID, 통화 코드, 외화 금액을 받아 처리
    public Payment prepare(Long orderId, String currency, BigDecimal foreignCurrencyAmount)
            throws IOException {
       
       BigDecimal exRate = getExRate(currency); //// 지정한 통화 코드로 환율 정보를 가져옴  
       BigDecimal convertedAmount = foreignCurrencyAmount.multiply(exRate); // 환율을 사용해 외화 금액을 원화 금액으로 변환
       LocalDateTime validUntil = LocalDateTime.now().plusMinutes(30);       // 결제 유효 시간을 현재 시간에서 30분 후로 설정
       // 결제 정보를 담은 Payment 객체 생성 후 반환
        return new Payment(orderId, currency, foreignCurrencyAmount, exRate, convertedAmount, validUntil);
    }

    // 환율 정보를 가져오는 메서드로 별도로 분리하였다.
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
 ..... 생략
}

```
