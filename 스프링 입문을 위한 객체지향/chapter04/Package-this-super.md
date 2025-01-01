# Package/this/super



# Package

- package 키워드는 네임스페이스를 만들어주는 역할을 합니다.
- 예를 들어서 고객 사업부에서 Customer클래스를 작성하고 마케팅 개발팀에서도 Customer클래스를 사용하면 두 개의 클래스는 이름 충돌이 발생합니다.
- 이럴때 고객 사업부.Customer , 마케팅.Customer이라고 하면  클래스끼리의 충돌이 발생하지 않습니다.



# this

- this는 객체가 자기 자신을 지칭할 때 사용하는 키워드 입니다.

``` java
public class Cat {

	int var = 10;

		void test(){
			int var = 20;

			System.out.println(var);
			System.out.println(this.var); // 객체의 var를 의미합니다
		}

	public static void main(String[] args) {
		Cat cat = new Cat();
		cat.test();
	}
}
```

- 지역변수와 속성(객체 변수, 정적 변수)의 이름이 같은 경우 지역변수가 우선됩니다.
- 객체변수와 이름이 같은 지역변수가 있는 경우 객체 변수를 사용하려면 this를 접두사로 사용해야 합니다.
- 정적 변수와 이름이 같은 지역변수가 있는 경우 정적변수를 사용하려면 클래스명을 접두사로 사용해야 합니다.



## super

- super는 바로 위 상위 클래스의 인스턴스를 지칭하는 키워드이다.

``` java
class Animal{
	void method(){
		System.out.println("동물");
	}
}

class Bird extends Animal{
	void method(){
		super.method(); // 상위 클래스의 인스턴스를 지칭함.
		System.out.println("조류");
	}
}


public class Driver {

	public static void main(String[] args) {
		Bird bird = new Bird();
		bird.method();

	}
	/**
	 * 동물
	 * 조류
	 * 
	 */
}

```

















