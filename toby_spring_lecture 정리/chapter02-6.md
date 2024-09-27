
인터페이스를 통하여 현재 코드는 그림과 같아졌다.

코드레벨의 의존관계
![image](https://github.com/user-attachments/assets/829bcf35-556a-4b0b-a176-157b64f9703c)

해당 부분에서 PaymentService는 코드상(코드레벨)으로는 ExRateProvider에 의존하는것 처럼 보이지만 실제로는 아래의 그림과 같다

![image](https://github.com/user-attachments/assets/e336303d-76d8-452a-b52e-77c820d0fea3)

왜 그럴까 PaymentService() 안에 아래의 코드가 있기 때문이다.

```
 public PaymentService(){
         this.exRateProvider = new WebApiExRateProvider();
     }
```
WebApiExRateProvider 대신에 SimpleExRateProvider가 들어가려먼 결국 코드를 수정해야한다.

즉 PaymentService가 어떤 오브젝트를 사용하는지 설정하는 과정이 필요한데 이 과정을
관계설정이라고 한다.

오브젝트 다이어그램이며
해당 관계를 런타임 의존관계라고 한다.
![image](https://github.com/user-attachments/assets/e336303d-76d8-452a-b52e-77c820d0fea3)


아래 그림의 붉은색 표시는 인터페이스를 구현할 클래스에서 어떤 걸 사용해야 될것인가를 지정하는건데 
이를 관계 설정 책임이라고 한다.
![image](https://github.com/user-attachments/assets/e336303d-76d8-452a-b52e-77c820d0fea3)

이 붉은색 관계를 (의존관계)를 설정하는 클래스를 분리하면 아래의 그림과 같이 되어지고 PaymentService의 앞단으로 옮겨진다 
이제 PaymentService에서는 WebApiExRateProvider 과 SimpleExRateProvider를 사용해야하는지를 알 필요가 없다.
![image](https://github.com/user-attachments/assets/e336303d-76d8-452a-b52e-77c820d0fea3)

코드는 아래와 같이 변경되어진다.



```
public class Client {
    public static void main(String[] args) throws IOException {
        PaymentService paymentService = new PaymentService(new SimpleExRateProvider());
       .... 이하 생략
    }
}


===============================

생성자로 주입받도록 한다.
어떤 ExRateProvider (인터페이스)를 구현한 클래스를 사용하는지 알 필요가 없어진다.
PaymentService
public PaymentService(ExRateProvider exRateProvider){
         this.exRateProvider = exRateProvider;
     }

```

![image](https://github.com/user-attachments/assets/43f32483-5961-4fa1-9c63-baf14a78cb0c)










