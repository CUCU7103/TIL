# 스프링 컨테이너와 의존관계 주입하기

bean은 오브젝트라고 생각해도 아무런 문제가 없다
조금 더 자세하게 보자면 오브젝트 중에서 애플리케이션을 구성하고 있는, 
**애플리케이션의 기능을 담당하고 제공하는 핵심 클래스의 오브젝트를 빈이라고 부릅니다.**

![image](https://github.com/user-attachments/assets/ca53251e-9d64-4de9-89be-16841ad62124)


아래 그림에서의 의존관계는 런타임에 의존하는 관계를 의미한다.
![image](https://github.com/user-attachments/assets/f9e6b717-2324-4066-bbb9-ebc7cfd9599b)

전체 애플리케이션이 어떻게 구성되어질 것이라는 정보 -> 구성정보 (Configuration) 
여기서는 ObjectFactory이다

```
public class Client {
    // main 메서드: 코드를 실행하는 진입점
    public static void main(String[] args) throws IOException {
        // 빈 팩토리 사용
        BeanFactory beanFactory = new AnnotationConfigApplicationContext(ObjectFactory.class);
        PaymentService paymentService =  beanFactory.getBean(PaymentService.class);
        // USD 통화의 50.7 단위 외화를 원화로 변환하고 결제 정보 생성
        Payment payment = paymentService.prepare(100L, "USD", BigDecimal.valueOf(50.7));
        // 결제 정보 출력
        System.out.println(payment);
    }
}

getBean(); 어떤 빈을 가져올지 선택
```


```
@Configuration // 구성정보를 담을 클래스다 빈 클래스의 간의 관계를 맺는것을 알려줌
public class ObjectFactory {
    @Bean // 빈으로 지정
    public PaymentService paymentService() {
        return new PaymentService(exRateProvider());
    }

    @Bean // 빈으로 지정
    public ExRateProvider exRateProvider(){
        return new SimpleExRateProvider();
    }

}


```

![image](https://github.com/user-attachments/assets/3a003c2e-30fe-43c9-92aa-a657bb3e9707)

PaymentService
WebApiExRateProvider / SimpleExRateProvider
런타임에서 애플리케이션을 구성해서 오브젝트로 동작하는 빈 클래스들

DI 의존관계 주입
- 외부에서 제 3의 오브젝트가 두 개의 실제 애플리케이션이 동작하는데 사용되어지는 오브젝트를 생성하고 그 의존관계를 맺어주는 것
- 이것을 dependency injection이라고 한다.
- 의존관계를 외부에서 주입해준다.










