# 오브젝트 팩토리

![image](https://github.com/user-attachments/assets/b2bb440e-523d-438d-87f4-b233c93463a4)

PaymentService가 가지고 있던 어떤 클래스의 오브젝트를 사용할 것인가에 대한 설정 책임을 Client로 넘겼다.
그래서 클라이언트가 PaymentService의 Object가 사용해야 할 클래스의 Object를 만들어서 설정하는 방식으로 해결하였다

여기서 클라이언트에 두 가지의 관심사가 생겼다.
자신만의 PaymentService를 이용해서 업무를 수행해야 하는데,
이때 PaymentService에서 어떤 클래스와 관계를 맺어야 할지에 대한 책임도 가지고 있다.

즉 두 가지의 관심사를 가졌고 관심사의 분리를 진행해야한다.

아래와 같이 분리해보자

![image](https://github.com/user-attachments/assets/c0c6d92b-82c4-4e9b-b59a-fb81e56a8682)

클라이언트는 PaymentService를 이용해서 payment를 준비하고 이를 위한 작업을 준비하는 책임에 충실하고
PaymentService의 사용준비는 새로운 클래스 ObjectFactory를 사용해서 준비하도록 하여 관심사를 분리하였다.


```
public class Client {
    public static void main(String[] args) throws IOException {
        ObjectFactory objectFactory = new ObjectFactory();
        PaymentService paymentService =  objectFactory.paymentService();// 어떤 Provider를 사용할 것인지 결정하는 책임이 있다.
        Payment payment = paymentService.prepare(100L, "USD", BigDecimal.valueOf(50.7));
        System.out.println(payment);
    }
}


===================================================================================================

public class ObjectFactory {

    public PaymentService paymentService() {
        return new PaymentService(exRateProvider());
    }

    public ExRateProvider exRateProvider(){
        return new WebApiExRateProvider();
    }

}



```
