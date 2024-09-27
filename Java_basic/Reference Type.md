## 자바의 기본형

![](https://github.com/user-attachments/assets/ad7c0484-0c8f-44ef-a8e3-fe121f7ed4bd)


위의 기본형 8가지를 제외한 나머지는 모두 참조 자료형이다.

기본 자료형과 참조 자료형의 **가장 큰 차이는 new를 사용**해서 객체를 생성하는지 여부의 차이이다.

조금 더 자세히 보자면

- 기본형은 변수에 사용할 값을 직접 넣을 수 있다.
- 참조형은 데이터에 접근하기 위한 참조(주소)를 저장하는 데이터 타입이다.

기본형 vs 참조형

- 기본형은 들어있는 값을 그대로 계산에 사용할 수 있다.
    - 더하고 빼고 등등 가능
- 참조형은 들어있는 참조값을 그대로 사용할 수 없다.
    
    
    - 더하고 빼고 사칙연산 사용 불가,
    

기본형은 연산이 가능하지만 참조형은 연산이 불가능하다.

```java
int a = 10, b = 20;
int sum = a + b;
```

**참조형은 변수에 객체의 위치인 참조값**이 들어있다. 참조값은 계산에 사용할 수 없다.

따라서 오류가 발생한다

```java
Student s1 = new Student();
Student s2 = new Student();
s1 + s2 //오류 발생
```

물론 다음과 같이
`.` 을 통해 객체의 기본형 멤버 변수에 접근한 경우에는 연산을 할 수 있다.

```java
Student s1 = new Student();
s1.grade = 100;
Student s2 = new Student();
s2.grade = 90;
int sum = s1.grade + s2.grade; //연산 가능
```

기본 생성자

- 매개변수가 없는 생성자
- 자바에서 자동으로 컴파일 시 생성한다.
- 기본 생성자는 다른 생성자가 있으면 자동으로 만들어지지 않는다.

자바에서 생성자가 필요한 이유 

- 자바에서 생성자는 자바 클래스의 객체(인스턴스)를 생성하기 위해서 존재한다.

### 클래스의 구조

```java
public class ReferenceString{
	
	String instanceVariable; // 인스턴스 변수
	public ReferenceString(){} // 생성자 영역
	public ReferenceString(String arg){} // 생성자 영역
	
	// 메서드 영역
	public static void main (String args[]){
		ReferenceString reference = new ReferenceString();
	}
	
	public String getString(){
		return instanceVariable;
	}

	public void setString(String str){
		instanceVariable = str;
	}
}
```

생성자를 만드는 갯수는 제한이 없다.

- 자바는 클래스의 객체를 보다 간편하게 만들기 위해서 여러 가지 매개변수를 갖는 생성자를  가질 수 있다.
- 생성자의 갯수는 1개든 100개든 상관없다.

DTO를 통해서 알아보자

Data transfer Object 의 약자이며 어떠한 속성을 갖는 클래스를 만들며 그것을 쉽게 전달하기 위함이다.

```java
public class MemberDTO{

	public String name; 
	public String phone;
	public String email;
	
	
	public MemberDTO getMemberDTO(){
		MemberDTO dto = new memberDTO();
		// 중간생략
		return dto;
			
	}

}

```

위와 같이 복합적인 데이터를 전달할때 유용하다.

여기서 여러 매개변수를 받는 생성자를 넣어본다면

```java
public class MemberDTO{

	public String name; 
	public String phone;
	public String email;

	public MemberDTO(){
	// 아무 정보도 모를때
	}
	
	// 이름만 앟때
	public MemberDTO(String name){
		this.name = name;
		
	}

    public MemberDTO(String name, String phone){
        this.name = name;
        this.phone = phone;
    }

    public MemberDTO(String name, String phone, String email){
        this.name = name;
        this.phone = phone;
        this.email = email;;
    }

}
	

	각각의 상황에 따른 생성자를 만들어서 사용할 수있다.
```

생성자의 사용 예시

```java

public class ReferenceConstructor
{
  
    public static void main(String[] args)
    {
        ReferenceConstructor reference = new ReferenceConstructor();
        reference.
    }

    public void makeMemberObject(){
        MemberDTO dto = new MemberDTO();
        MemberDTO dto1 = new MemberDTO("Sangmin");
        MemberDTO dto2 = new MemberDTO("Sangmin","010XXXXYYYY");
        MemberDTO dto3 = new MemberDTO("Sangmin","010XXXXYYYY","test@gmail");
    }
}
```

### this

- 객체의 변수와 매개변수를 구분하기 위한 예약어

```java
public class MemberDTO{

	public String name; 
	public String phone;
	public String email;

	public MemberDTO(String name){
		this.name = name;
		
	}
```

만일 this 를 사용하지 않는다면 

```java
public class MemberDTO{

	public String name; 
	public String phone;
	public String email;

	public MemberDTO(String name){
		name = name;
		
	}
```

자바 컴파일러에서 변수를 구분하지 못해서 에러가 발생할 수 있다.

이러한 혼동을 방지하기 위해서 this를 사용하여 자바의 변수와 매개변수를 구분한다.

this를 사용해서 구분하게되면  “해당 객체의 변수이다.” 라고 쉽게 알 수가 다.

### Method Overloading

- 메소드의 이름은 동일하지만 각 메소드의 매개변수의 종류와 개수, 순서를 달리하여 생성할 수있다.
- 이와 같은 방식으로  메소드의 이름을 동일하게 하고 , 매개변수만을다르게 하는 것을 메서드 오버로딩(Method Overloading)이라고 한다.
- “같은 역할을 하는 메서드는 같은 메서드 이름을 가져야 한다.”는 모토로 사용된다고 생각하면 이해하기 편하다.

```java

public class ReferenceOverloading
{
    
    public static void main(String[] args)
    {
      ReferenceOverloading reference = new ReferenceOverloading();

    }

    public void print(int data){

    }

    public void print(String data){

    }

    public void print(int intData,String stringData){

    }

    public void print(String stringData , int intData){
        
    }

}
```

### 메서드의 생명주기

자바에서 메서드가 종료되어지는 조건

- 메서드의 모든 문장이 실행되었을때
- return 문장에 도달했을때
- 예외가 발생했을때(throw)

- return을 통해서 값을 반환하고 메서드를 종료

```java
// tip: each public class is put in its own file
public class ReferenceReturn
{
    // tip: arguments are passed via the field below this editor
    public static void main(String[] args)
    {
 
    }

    public int intReturn(){
        int returnInt = 0;
        return returnInt;
    }

    public int[] intArrayReturn(){
        int returnArray[] = new int[10];
        return returnArray;
    }

    public String stringReturn(){
        String returnString = new "Return Value";
        return returnString;
    }

}
```

return 이라는 예약어를 사용하여 메서드를 리턴한면 그 메서드는 값을 넘겨주고 그 위치에서 종료되어진다.

return 으로 값을 넘기고 그 뒤에 아무런 문장이 오면 안된다

단 if문은 예외이다.

```java
public int ifConditionNoElseIntReturn(){
	
	int returnInt =0;
	if(returnInt == 0){
		return ++returnInt;
	}else{
		return --returnInt;
	}
}
```

if문 안에 return 문장이 있을경우에는 그 이외의 경우가 존재하기 때문에 리턴문장이 하나의 메서드 안에 두 개 이상 있어야 한다.

리턴 타입이 void인 메서드에서 더 이상 실행하고 싶지 않을때에는 어떻게 하면 좋을까?

```java
public void wantToStopInTheMiddle(boolean flag){
		//... 위에 많은양의 코드가 있다고 가정함.
		if(flag) return;
		// .. 아래애도 많은 양의 코드가 있다고 가정.
}
```

- return 뒤에 아무것도 없이 바로 세미콜론을 적어주면 메서드 수행을 종료해라 라고 인식한다.

### Static 메서드와 일반 메서드의 차이

일반적으로 메서드를 사용하려면 객체를 생성하고 호출하였는데

static 메서드는 객체를 생성하지 않고 바로 사용이 가능하다

```java

public class ReferenceStatic{

  public static void main(String[] args){
        ReferenceStatic.stacticMethod();
   }

    
    public static void stacticMethod(){
        System.out.println("This is a staticMethod");
    }

}
   
  
```

static 메서드는 인스턴스 변수를 받을 수 없다. 

클래스 변수 (static 변수)만 받을 수있다.

why? 자바의 메모리 영역에 올라가는 시점이 다르기 때문이다.

클래스(static) 변수는 모든 객체에서 하나의 값을 바라본다.

예시 

```java
package test;

public class ReferenceStaticVariable {
    static String name;
    public ReferenceStaticVariable(){}
    public ReferenceStaticVariable(String name){
        this.name = name;
    }

    // tip: arguments are passed via the field below this editor
    public static void main(String[] args) {
        ReferenceStaticVariable reference = new ReferenceStaticVariable();
        reference.checkName();
    }

    public void checkName(){
        ReferenceStaticVariable reference1 = new ReferenceStaticVariable("park");
        System.out.println(reference1.name);
        ReferenceStaticVariable reference2 = new ReferenceStaticVariable("kim");
        System.out.println(reference1.name);
    }
}

```

- 위 코드는 클래스 변수 name을 각각 호출하였다.
- 예상하기로 동일한 클래스 변수를 호출했으니 park, park이 나와야 할것같지만
- 실제로는 park, kim이 출력되어진다.
    - 자바의 메모리 구조로 위의 문제를 설명할 수있다 이는 별도로 정리할 예정이다.

우리가 원하는 값을 출력하도록 하려면 변수에 static을 제거하면 깔끔하게 나온다.

### Static 블록

- 객체는 여러개를 생성하지만 한 번만 호출되어야 하는 코드가 있다면 static 블록을 사용하면 된다.

```java
static {
	// 딱 한 번만 수행되어지는 코드
	
}
```

- static 블록은 객체가 생성되기 전에 한번 만 호출되고, 그 이후에는 호출하려고 해도 호출 할 수가 없다.
- 클래스 내에 선언되어 있어야 하며 메소드 내에서는 선언할 수가 없다.

```java
package test;

public class StaticBlock {

    static int data = 1;
    public StaticBlock(){
        System.out.println("StaticBlock Constructor");
    }

    // 생성자가 호출되기 전 먼저 수행되어진다.
    static {
        System.out.println("***First Static block***");
        data = 3;
    }

    static {
        System.out.println("***Second Static block***");
        data = 5;
    }

    public static int getData(){
        return data;
    }

}

==============================================================================

package test;

public class StaticBlockCheck {

    public static void main(String[] args) {
        StaticBlockCheck check = new StaticBlockCheck();
        check.makeStaticBlockObject();
    }

    public void makeStaticBlockObject() {
        System.out.println("Create block1");
        StaticBlock block = new StaticBlock();
        System.out.println("create block1");
        System.out.println("-----------------");
        System.out.println("Create block2");
        StaticBlock block2 = new StaticBlock();
        System.out.println("Create block2");
    }
}

수행 결과

Create block1
***First Static block*** // static 블럭이 먼저 수행되는 것을 확인할 수 있다.
***Second Static block*** // static 블럭이 먼저 수행되는 것을 확인할 수 있다.
StaticBlock Constructor
create block1
-----------------
Create block2
StaticBlock Constructor
Create block2
```

- 즉 static 블럭은 클래스를 초기화 할때 꼭 수행되어야 하는 작업이 있을 경우 유용하게 사용할 수 있다.
- static 블럭 안에는 static 한 것들만 호출 될 수있다.

기본형과 참조형의 변수 대입 

- 대원칙 자바는 항상 변수의 값을 복사해서 대입한다.

### 기본형의 변수 대입

```java
public class VarChange1 {
public static void main(String[] args) { 
int a = 10;
int b = a;
System.out.println("a = " + a); // 10
System.out.println("b = " + b); // 10
//a 변경 
a = 20;
System.out.println("변경 a = 20");
System.out.println("a = " + a); // 20 
System.out.println("b = " + b); // 10
//b 변경
b = 30;
System.out.println("변경 b = 30");
System.out.println("a = " + a);
System.out.println("b = " + b);
} 

}

// 실행 결과
a = 10
b = 10
변경 a = 20
a = 20
b = 10
변경 b = 30
a = 20
b = 30
```

![image](https://github.com/user-attachments/assets/4b6c2014-c1b1-4406-84b0-e4d38712e983)

a값이 변경되었을때

![image](https://github.com/user-attachments/assets/3f9deb6e-a1ac-4cab-b24f-fd71c050ff8a)

### 참조형의 변수 대입

```java
public class VarChange2 {
public static void main(String[] args) { 
Data dataA = new Data();
dataA.value = 10;
Data dataB = dataA;
System.out.println("dataA 참조값=" + dataA);
System.out.println("dataB 참조값=" + dataB);
System.out.println("dataA.value = " + dataA.value);
System.out.println("dataB.value = " + dataB.value);
//dataA 변경 dataA.value = 20;
System.out.println("변경 dataA.value = 20");
System.out.println("dataA.value = " + dataA.value);
System.out.println("dataB.value = " + dataB.value);
//dataB 변경 dataB.value = 30;
System.out.println("변경 dataB.value = 30");
System.out.println("dataA.value = " + dataA.value);
System.out.println("dataB.value = " + dataB.value);
}
}

// 실행 결과
dataA 참조값=ref.Data@x001
dataB 참조값=ref.Data@x001
dataA.value = 10
dataB.value = 10
변경 dataA.value = 20
dataA.value = 20
dataB.value = 20
변경 dataB.value = 30
dataA.value = 30
dataB.value = 30
```

dataA 인스턴스 생성

![image](https://github.com/user-attachments/assets/d251267b-6ef4-416b-b76e-708b9b71826c)

![image](https://github.com/user-attachments/assets/b03a9630-9143-440b-95a4-cd11143b666d)

dataA.value의 값을 변경할때 참조하고 있는 인스턴스의 값을 변경한다.

dataB.value 또한 동일한 인스턴스를 참조하고 있기 때문에 값이 변경되어진다.

![image](https://github.com/user-attachments/assets/3d4e61a4-d76f-4275-aae4-1913806c5b73)

![image](https://github.com/user-attachments/assets/f6a4fb41-df3a-48fa-ae12-e38c662213d9)


```java
package test;

public class ReferencePass {

    public static void main(String[] args) {
        ReferencePass pass = new ReferencePass();
//        pass.callByPassValue();
        pass.callPassByReference();
    }

    public void callByPassValue() {
        int a = 10;
        String b = "b";
        System.out.println("before passByValue");
        System.out.println("a = " + a);
        System.out.println("b = " + b);
        passByValue(a,b);
        System.out.println("after passByValue");
        System.out.println("a = " + a);
        System.out.println("b = " + b);
    }

    public void passByValue(int a , String b){
        a = 20;
        b = "z";
        System.out.println("in passByValue");
        System.out.println("a =" + a);
        System.out.println("b =" + b);

    }

    public void callPassByReference(){
        MemberDTO member = new MemberDTO("Sangmin");
        System.out.println("before passByReference");
        System.out.println("member.name = " + member.name);
        // 메서드의 매개변수로 참조 자료형을 넘길 경우에는 메서드안에서 객체의 상태를 변경한 결과에 영향을 받는다.
        passByReference(member);
        System.out.println("after passByReference");
        System.out.println("member.name " + member.name);
    }

    public void passByReference(MemberDTO member) {
        member.name = "ParkJoon";
        System.out.println("in passByReference");
        System.out.println("member.name " + member.name);
    }
}

// 결과 
before passByValue
a = 10
b = b
in passByValue
a =20
b =z
after passByValue
a = 10
b = b
before passByReference
member.name = Sangmin
in passByReference
member.name ParkJoon
after passByReference
member.name ParkJoon
```
