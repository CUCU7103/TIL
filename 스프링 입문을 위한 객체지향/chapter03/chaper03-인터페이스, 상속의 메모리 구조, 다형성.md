# chaper03-인터페이스, 상속의 메모리 구조, 다형성

- 자바에서는 다중 상속대신에 인터페이스를 사용한다.

- 상속이 is a kind of라면 인터페이스는 be able to 라고 할 수있다.



### 인터페이스는 "무엇을 할 수 있는"이라는 표현 형태로 만드는 것이 좋다.

- 자바 api서도 이러한 형식의 인터페이스르 볼 수 있다.
  - Serializable 인터페이스 : 직렬화 할 수있는
  - Cloneable 인터페이스 : 복제할 수 있는
  - Comparable 인터페이스 : 비교할 수 있는

- 인터페이스는 클래스가 ''무엇을 할 수있다'' 라고 하는 기능을 구현하도록 강제합니다.



## 상속과 메모리 구조

```  java
public class Animal {
	public String name;

	public void showName(){
		System.out.printf("안녕 나는 %s야. 반가워\n",name);
	}
}


===========================================
    
public class Penguin extends Animal{
	public String habitat;

	public void showHabitat(){
		System.out.printf("%s는 %s에 살아\n", name, habitat);
	}

}    
    

============================================
    
 public class Driver {
	public static void main(String[] args) {
		Penguin pororo = new Penguin();

		pororo.name = "뽀로로";
		pororo.habitat = "남극";

		pororo.showName();
		pororo.showHabitat();

		Animal pingu = new Penguin();
		pingu.showName();
	// 	pingu.shoHabitat();

		pingu.showName();
		// pingu.shoHabitat();

		// Penguin happyfeet = new Animal();

	}
}
    
    

```

![image-20241225183118241](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20241225183118241.png)

- 그림을 보면 알 수 있듯이 Penguin 클래스의 인스턴스만 힙 영역에 생긴 게 아니라 Animal 클래스의 인스턴스도 함께 힙 영역에 생긴것을 볼 수 있습니다.
  - 즉 하위 클래스의 인스턴스가 생성되어지면 상위 클래스의 인스턴스도 생성되어집니다.
  - 모든 클래스의 상위 클래스인 Object 클래스의 인스턴스도 함께 생성되어집니다.

![image-20241225183654645](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20241225183654645.png)

- Penguin pororo = new Penguin();
- Animal pingu = new Penguin();
  - 상위 클래스는 하위 클래스의 인스턴스를 포함할 수 있습니다.
  - **pingu 객체 참조 변수는 Animal 클래스를 참조하고 있기에 Penguin 클래스의 속성과 메서드를 사용할 수 없다.**



### 다형성 - 사용편의성

- 객체지향에서 다형성이라고 하면 오버라이딩과 오버로딩이라고 할 수있습니다.
- 물론 상위 클래스와 하위 클래스 사이에서도 다형성을 이야기 할 수 있고 인터페이스와 그 구현 클래스 사이에서도 다형성을 이야기 할 수있습니다.



- **오버라이딩**
  - 같은 메서드 이름, 같은 인자 목록으로 상위 클래스의 메서드를 재정의
- **오버로딩**
  - 같은 메서드 이름, 다른 인자 목록으로 다수의 메서드를 중복정의



```JAVA

public class Animal {
	public String name;

	public void showName(){
		System.out.printf("안녕 나는 %s야. 반가워\n",name);
	}
}


================================================
package Inheritance04;

public class Penguin extends Animal {
	public String habitat;

	public void showHabitat(){
		System.out.printf("%s는 %s에 살아\n", name, habitat);
	}

	// 오버라이딩
	public void showName(){
		System.out.println("제 이름은 몰라도 되요");
	}

	// 오버로딩 - 중복정의: 같은 메서드 이름, 다른 인자 리스트
	public void showName(String yourName){

		System.out.printf("%s 안녕, 나는 %s라고 해\n", yourName, name);

	}

}

==================================================

public class Driver {
	public static void main(String[] args) {
		Penguin pororo = new Penguin();

		pororo.name = "뽀로로";
		pororo.habitat = "남극";

		pororo.showName();
		pororo.showName("초보");
		pororo.showHabitat();

		Animal pingu = new Penguin();
		pingu.name= "핑구";
		pingu.showName();

	}
}
```



![image-20241225191129595](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20241225191129595.png)

- 그림을 통해서 확인할 수 있는 부분은 Penguin 클래스가 상위 클래스인 Animal 클래스의 showName() 메서드를 오버라이딩 했다는 것과 showName(yourName String) 오버로딩 했다는 것입니다.
- 즉 **상위 클래스 타입의 객체 참조 변수를 사용하더라도 하위 클래스에서 오버라이딩 한 메서드가 호출되어집니다.**



