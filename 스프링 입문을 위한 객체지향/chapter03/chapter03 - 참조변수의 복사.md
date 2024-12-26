## 참조 변수의 복사

- 기본 자료형 변수를 복사하는 경우 Call by value에 의해 그 값이 복사되며 두 개의 변수는 서로에게 영향을 주지 않는다.
- 참조 변수를 복사하는 경우는 저장하고있는 값을 주소로 해석한다.
- **즉 기본 자료형 변수는 저장하고 있는 값을 그 값 자체로 해석하는 반면, 객체 참조 변수는 저장하고 있는 값을 주소로 해석하는 것입니다.**



``` java
public class CallByValue {

	public static void main(String[] args) {
		int a = 10;
		int b = a;
		//  변수에 값을 복사한다ㅏ
		b = 20;

		System.out.println(a);
		System.out.println(b);
	}
}

// 10
// 20

======================================================
    
   public class CallByReference {

	public static void main(String[] args) {
		Test1 test1 = new Test1();
		Test1 test2 = test1;

        // 같은 객체를 참조하고 있다.
        // 변수에 값을 복사하는 것은 동일하다.
		test1.a = 10;
		test2.a = 30;

		System.out.println(test1.a);
		System.out.println(test2.a);
	}


}
 
// 30
// 30
    
    
    
```

- 기본 자료형 변수는 값을 값 자체로 판단합니다.
- 참조 자료형 변수는 값을 주소 , 포인터로 판단합니다.
- 기본 자료형 변수를 복사할때, 참조 자료형 변수를 복사할때 일어나는 일은 같습니다.
  - 즉 가지고 있는 값을 그대로 복사해서 넘겨주는 것입니다.

