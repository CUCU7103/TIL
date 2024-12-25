# chapter03 상속의 강력함

``` java
package Inheritance;

public class Animal {
	String name;

	public void showName(){
		System.out.println(name);
	}

}


public class Bird extends Animal{
	String habitat;

	public void fly(){
		System.out.println(habitat);
	}
}


public class DriverClass {
	public static void main(String[] args) {
		Bird bird = new Bird();
		bird.name = "파랑새";
		bird.habitat = "빠른 비행";

		bird.showName();
		bird.fly();
	}

}


```

- 상위 클래스에서만 showName()메서드를 구현했지만 하위 클래스에서 사용할 수있습니다.
  - 상속한다는 것이 상위 클래스의 특징을 상속한다는 의미이지 부모-자식 관계는 아닙니다.



``` java
public class DriverClass02 {
	public static void main(String[] args) {
		Animal animal = new Animal();
		Animal bird = new Bird();

		animal.showName();
		bird.showName();

	}

}
```

- 위 예시를 통해서 **하위 클래스는 상위 클래스다,** 혹은 하위 분류는 상위 분류이다. 라는 말이 코드에서 어떻게 표현되는지 알 수있다.

- 또한 클래스 상속 구조에서 가장 최상위에 위치한 클래스는 Object 클래스 입니다.
  - 그래서 모든 클래스는 Object의 특성을 물려받고 , 클래스에 구현받지 않고 Object의 메서드인 toString()을 사용할 수 있습니다.



## 상속은 is a 관계를 만족해야 한다?

- is a 관계는 객체와 클래스의 관계로 오해될 소지가 많습니다.
  - 예시
    - 객체 is a 클래스
    - 김연아 is a 사람  -> 김연아는 한 명의 사람이다.
    - 뽀로로 is a 펭귄 -> 뽀로로는 한 마리 펭귄이다
- 좀 더 명확한 표현으로는 **is a kind of** 관계를 사용할 수있습니다.
  - 펭귄 is a kind of 동물 -> 펭귄으 동물의 한 분류다.
  - 고래 is a kind of 동물 -> 고래는 동물의 한 분류다.
  - 조류 is a kind of 동물 -> 조류는 동물의 한 분류다.



### 상속과 관련해서는 아래의 3가지 문장을 기억하자

- 객체 지향의 상속은 상위 클래스의 특성을 재사용하는 것이다.
- 객체 지향의 상속은 상위 클래스의 특성을 확장하는 것이다.
- 객체 지향의 상속은 is a kind of 관계를 만족하는 것이다.



