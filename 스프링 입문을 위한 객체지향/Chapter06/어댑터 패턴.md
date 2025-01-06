# 디자인 패턴

- 디자인 패턴은 객체 지향의 특성 중 상속, 인터페이스, 합성을 이용합니다.



## 어댑터 패턴

- 어뎁터를 변환하면 변환기라고 할 수 있습니다.
- 변환기의 역할은 서로 다른 두 인터페이스 사이에 통신이 가능하게 하는 것 입니다.
- 어댑터 패턴은 개방 폐쇄 원칙(OCP)를 설명할 때도 예로 들었던 내용입니다.

```java
public class ServiceA {

	void runServiceA() {
		System.out.println("ServiceA");
	}

}

===================================

public class ServiceB {

	void runServiceB(){
		System.out.println("ServiceB run service");
	}

}

====================================

public class ClientWithNoAdapter {
	public static void main(String[] args) {
		ServiceA sa1 = new ServiceA();
		ServiceB sa2 = new ServiceB();

		sa1.runServiceA();
		sa2.runServiceB();

	}
}

```

- 어댑터 패턴을 적용하기 전의 시퀀스 다이어그램

![image-20250106232156476](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20250106232156476.png)

``` java
public class AdapterServiceA {
	ServiceA sa1 = new ServiceA();

	void runService(){
		sa1.runServiceA();
	}

}
=================================
 public class AdapterServiceB {
	ServiceB sa2 = new ServiceB();

	void runService(){
		sa2.runServiceB();
	}

}   
==================================
 public class ClientWithAdapter {
	public static void main(String[] args) {
		AdapterServiceA asa1 = new AdapterServiceA();
		AdapterServiceB asa2 = new AdapterServiceB();

		asa1.runService();
		asa2.runService();

	}
}
=========================   
```

- 클라이언트가 변환기를 통해 runService()라는 동일한 메서드명으로 두 객체의 메서드를 호출하는 것을 볼 수 있습니다.

![image-20250106234346102](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20250106234346102.png)

- 즉 **어댑터 패턴은 합성, 객체를 속성으로 만들어 참조하는 디자인 패턴**입니다.
- 한 문장으로 정리해보자면, 
  - "**호출당하는 쪽의 메서드를 호출하는 쪽의 코드에 대응하도록 중간에 변환기를 통해 호출하는 패턴"**입니다.
