
빈을 좀 더 쉽게 사용할 수있도록 할 수 있습니다.

오브젝트 팩토리를 통해서 스프링 컨테이너가 얻을 수 있는 정보를 **스프링 컨테이너가 직접 획득할 수 있게 변경할 수 있다.**

기존에 오브젝트 팩토리를 통해서 얻은 정보
1. 어떤 클래스가 빈을 만들 것인가
2. 그 클래스가 인터페이스를 통해서 의존하고 있는 오브젝트의 클래스는 무엇인가

해당 정보를 확실하게 알 수 있다면 @Bean을 만들어 내지 않아도 된다

@Bean을 제거하고 
```
@Configuration // 구성정보를 담을 클래스다 빈 클래스의 간의 관계를 맺는것을 알려줌
@ComponentScan
public class ObjectFactory {
  /*  @Bean
    public PaymentService paymentService() {
        return new PaymentService(exRateProvider());
    }

    @Bean
    public ExRateProvider exRateProvider(){
        return new SimpleExRateProvider();
    }*/

}

```

@Bean의 대상이 되었던 클래스에 @Component를 붙이면 된다.
@Component -> 이 애플리케이션의 빈 오브젝트가 될 대상임을 의
```
@Component
 public class PaymentService {
    // 클래스 생성시 한번만 만들어지도록 함.
     // 인터페이스르 구현한 클래스는 인터페이스 타입의 변수에 담아둘 수있다.
    private final ExRateProvider exRateProvider;

     public PaymentService(ExRateProvider exRateProvider){
         this.exRateProvider = exRateProvider;
     }

..........................

}

============================================================================

@Component
public class SimpleExRateProvider implements ExRateProvider{

    @Override
    public BigDecimal getExRate(String currency) throws IOException {
        if(currency.equals("USD")) return BigDecimal.valueOf(1000);
        throw new IllegalArgumentException("지원되지 않는 통화입니다.");
    }
}

```


기존 오브젝트 팩토리는 아래와 같이 변경되어진다.
명시적으로 어떻게 구성이 될지 정보를 주지 않지만 알아서 찾아서 구성정보를 찾는다.
@ComponentScan // @Component 가 붙은 클래스를 스캔해서 구성 정보를 찾습니다.
```
@Configuration // 구성정보를 담을 클래스다 빈 클래스의 간의 관계를 맺는것을 알려줌
@ComponentScan // @Component 가 붙은 클래스를 스캔해
public class ObjectFactory {
  /*  @Bean
    public PaymentService paymentService() {
        return new PaymentService(exRateProvider());
    }

    @Bean
    public ExRateProvider exRateProvider(){
        return new SimpleExRateProvider();
    }*/
}



```















