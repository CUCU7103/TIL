# Chapter03 자바와 객체 지향 - 추상화02

- mouse 클래스를 만들어보자 

``` java
public class  Mouse {
    
   	public String name;
    
  	 public int age
         
    public int countOfTail;
    
    
    public void sing() {
		
        System.out.println(name + "찍찍!");
    	
    }
       
}

======================
    
    package abstraction;

public class MouseDriver {
	public static void main(String[] args) {
		Mouse micky = new Mouse();
		micky.name = "미키";
		micky.age = 85;
		micky.countOfTail = 1;

		micky.sing();

		micky = null;

		Mouse jerry = new Mouse();

		jerry.name = "제리";
		jerry.age = 73;
		jerry.countOfTail = 1;

		jerry.sing();

	}
}

```



![image-20241222143757277](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20241222143757277.png)

- MouseDriver의  main() 메서드 부분에서의 jvm 메모리 영역의 구조는 위의 그림과 같다.
  - java.lang  패키지와 모든 클래스들이 스테틱 영역에 배치되어집니다.

- 이 위 그림에서 현재 mouse에서는 변수 저장공간이 보이지 않습니다.
- 위 세개의 속성 name,age countOfTail은 클래스에 속한 속성이 아닌 Mouse 객체에 속한 속성이기 때문이다.
- 객체가 생성되어야지만 속성의 값을 저장하기 위한 메모리 공간이 힙 영역에 할당됩니다.

- 그리고 MouseDriver 클래스의 main() 메서드에서는 밑줄이 그어져 잇고 Mouse 클래스의 sing() 메서드에는 밑줄이 없습니다.
  - main() 메서드는 클래스의 멤버 메서드이고 , sing()은 객체의 멤버 메서드이기 때문입니다.
  - 클래스 멤버와 객체 멤버를 구분하는 자바 키워드는 static 입니다.

- `Mouse micky` 
  - Mouse 객체에 대한 참조 변수 micky를 만든다.
- `new Mouse()` 
  - 클래스의 인스턴스를 하나 만들어 힙에 배치한다.
- `Mouse micky = new Mouse();` 
  - Mouse 객체에 대한 주소를 참조변수 micky에 할당한다.



![image-20241222163158785](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20241222163158785.png)

- new Mouse 까지 실행된 후의 jvm 메모리 영역의 구조이다.

![image-20241222163355445](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20241222163355445.png)

- 객체 참조변수 micky가 Mouse 객체에 대한 참조 변수라는 것을 알 수있다.

![image-20241222163526402](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20241222163526402.png)

- micky.countOfTail = 1; 이 부분까지 실행 후의 메모리 영역을 표현한 그림

- `micky = null;`  이 부분에서는 더 이상 객체 참조변수 micky가  힙 영역에 존재하는 Mouse 객체를 참조하지 않는다.
- 이럴때 GC가 발생하여 힙 영역에서 더 이상 참조되지 않는 객체 mouse를 제거합니다.
- 이후 jerry 또한 위와 동일한 방식으로 진행됩니다.







