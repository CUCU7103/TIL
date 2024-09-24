![image](https://github.com/user-attachments/assets/6941d849-2fa5-4319-891a-61a5d0935826)

상속이라는 것은 상위 클래스와 하위 클래스가 매우 강하게 결합되어져 있다.<br>
단일 상속만 가능하다. 

클래스의 분리
상속이 아니라 아예 별도의 클래스로 변경하였다.

```
public class PaymentService {
    // 클래스 생성시 한번만 만들어지도록 함.
    private final SimpleExRateProvider exRateProvider;

     public PaymentService(){
         this.exRateProvider = new SimpleExRateProvider();
     }

     // 결제 준비 메서드: 주문 ID, 통화 코드, 외화 금액을 받아 처리
    public  Payment prepare(Long orderId, String currency, BigDecimal foreignCurrencyAmount)
            throws IOException {

        BigDecimal exRate = exRateProvider.getExRate(currency); // 원화 환율
        BigDecimal convertedAmount = foreignCurrencyAmount.multiply(exRate); // 환율을 곱하여 원화 금액으로 변환
        LocalDateTime validUntil = LocalDateTime.now().plusMinutes(30); // 유효시간
        return new Payment(orderId, currency, foreignCurrencyAmount, exRate, convertedAmount, validUntil);
    }



}

public class WebApiExRateProvider {


    BigDecimal getWebExRate(String currency) throws IOException {
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

            // 환율 반환
            return data.rates().get("KRW");
    }
}

```
단 이렇게 별도의 클래스로 분리하였을 때 발생하는 문제점이 있다. 

BigDecimal exRate = exRateProvider.getExRate(currency); // 원화 환율

이 부분에서 클래스가 변경될때마다 메소드를 수정해야 한다
만일 저렇게 수정할 부분이 수십개가 있으면 머리가 아파온다...
