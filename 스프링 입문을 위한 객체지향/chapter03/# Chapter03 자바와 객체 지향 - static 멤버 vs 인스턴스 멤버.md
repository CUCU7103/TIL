# Chapter03 자바와 객체 지향 - static 멤버 vs 인스턴스 멤버



- 같은 클래스의 모든 객체(인스턴스)가 같은 값을 가지고 있다면  그 값을 클래스에 저장하여 사용하는 것이 좋지 않을까?

![image-20241222164313109](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20241222164313109.png)



``` java
public class Mouse {

	public String name;
	public int age;
	public static int countOfTail = 1; // static 변수 선언

	public void sing(){
		System.out.println(name + " 찍찍");
	}

}
```

- 이제 countofTail 변수에 접근하기 위해서는 객체를 이용해 객체참조변수.countofTail로 접근이 가능하고
- 클래스명.countofTail로도 접근이 가능하다.



### static 키워드가 붙은 속성을 클래스 맴버 속성이라고 합니다.

- 마찬가지로 메서드에도 static이 붙는다면 클래스 맴버 메서드 라고 합니다.



정리해보자면 

- 클래스 맴버 = static 맴버 = 정적 맴버

- 객체 맴버 = 인스턴스 맴버

- ### static 맴버 변수(속성)을 사용할때

  - 해당 클래스의 모든 객체가 같은 값을 가질때 사용하는 것이 기본입니다.

- ### static 맴버 메서드를 사용할때

  - 객체들의 존재 여부에 관계없이 사용할 수 있는 메서드입니다.
    - main() 메서드는 정적메서드여야 합니다.
  - 유틸리티성 메서드
  - getter와 setter 
  - 실무에서는 클래스의 객체를 만들지 않고 사용할 수있게 해주는 유틸리티성 메서드에 사용



| 이름          | 다른이름                                 | 위치        |
| ------------- | ---------------------------------------- | ----------- |
| static 변수   | 클래스[맴버] 속성, 정적 변수 , 정적 속성 | 스태틱 영역 |
| 인스턴스 변수 | 객체[멤버] 속성, 객체 변수,...           | 힙 영역     |
| local 변수    | 지역 변수                                | 스택 영역   |





추상화를 통해서 모델링을 하게 되면

- 클래스 설계
  - 클래스 멤버
    - static
      - 클래스 맴버 속성
      - 클래스 맴버 메서드
  - 객체 멤버
    - 객체 맴서 속성
    - 객체 맴버 메서드
