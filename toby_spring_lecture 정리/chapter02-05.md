상속의 단점을 극복하고 클래스의 분리를 유지하기 위해서 인터페이스를 사용한다.
인터페이스는 구현한다 라는 단어를 사용한다.

![image](https://github.com/user-attachments/assets/0b99ce98-a06e-4b82-ae7d-3eaab661b969)


```
 인터페이스 생성
public interface ExRateProvider {
    BigDecimal getExRate(String currency) throws IOException;
}

```
WebApiExRateProvider의 변경
```
public class WebApiExRateProvider implements ExRateProvider {

    @Override
    public BigDecimal getExRate(String currency) throws IOException {

            URL url = new URL("https://open.er-api.com/v6/latest/" + currency);
            HttpURLConnection connection = (HttpURLConnection) url.openConnection();
            BufferedReader br = new BufferedReader(new InputStreamReader(connection.getInputStream()));

            String response = br.lines().collect(Collectors.joining());
            br.close();

            ObjectMapper mapper = new ObjectMapper();
            ExRateData data = mapper.readValue(response, ExRateData.class);

            return data.rates().get("KRW");

    }
}


```


기존의 PaymentService의
```
BigDecimal exRate = exRateProvider.getExRate(currency); 이 부분이 개선되었다
클래스에 따라서 메서드를 계속 변경 시켜주어야 했는데 인터페이스를 사용하면서 하나의 메서드명으로 사용할 수 있게 됨

```

하나의 클래스에서 사용하는 즉 의존하는 다른 어떤 클래스들이 있을 때 그 안에 인터페이스를 정의해놓고
인터페이스 타입으로 사용하게하면 코드의 변경을 최소화 할 수있다.

인터페이스를 통한 다형성이다.


하지만 PayMentService 의 아래 부분에서 강하게 의존하고 있다
```
 public PaymentService(){
         this.exRateProvider = new WebApiExRateProvider();
     }
```
해당 부분을 계속 변경해 줘야하는 불편함이 있다.

이 부분을 유연하고 느슨하게 만들어줘야 한다.







