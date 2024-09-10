# 변수

변수에는 4가지 종류가 존재합니다.
- 지역변수
  - 중괄호 내에서 선언된 변수
  - 지역변수를 선언한 중괄호 내에서만 유효하다.
 
- 매개 변수
  - 메서드에 넘겨주는 변수
  - 메서드가 호출되어질때 생명이 시작되고 메서드가 끝나면 소멸됨.
    
- 인스턴스 변수
  - 메소드 밖에, 클래스 안에 선언된 변수, 앞에는 static이라는 단어가 없어야 한다.
  - 객체가 생성될 때 생명이 시작되고 그 객체를 참조하고 있는 다른 객체가 없으면 소멸된다.
    
- 클래스 변수
  - 인스턴스 변수처럼 메서드 밖에 클레스 안에 선언된 변수들 중에서 타입 선언 앞에 static이라는 예약어가 있는 변수
  - 클래스가 처음 호출 될때 생명이 시작되고, 자바 프로그램이 끝날때 소멸된다.
  
<br>

조금 더 명확하게 이해하기 위해 코드로 보자면

``` java
public class VariableTypes{
  int instanceVariable;
  static int classVariable;
  public void method(int parameter){
    int localVariable;
  }

}

``` 

**지역변수에 관한 예제**

```java

public class VariableTypes{
  int instanceVariable;
  static int classVariable;

  public void method(int parameter){
    int localVariable;
  }
  // 다른 메서드에 동일한 이름의 지역변수 생성함.
  public void anotherMethod(){
    int localVariable;
  }

}


```

- 지역변수는 지역변수를 선언한 중괄호 내에서만 유효하기 때문에 두 변수는 서로 다른 지역변수이다.
- 그러기에 아래 코드에서의 지역변수도 서로 다른 지역변수이다.

```java
public void anotherMethod(){
  if(true){
    int localVariable;
  }

   if(true){
    int localVariable;
  }
  
}
```

**지역변수의 잘못된 사용의 예**

```java
public void anotherMethod(){
  if(true){
    int localVariable;
   if(true){
    int localVariable; ==> 잘못된 지역변수의 사용
    }
  }

if(true){
    int localVariable;
  }
}
```
- 두 번째로 사용한 지역변수는 첫 번째 localVariable을 선언한 중괄호 안에 같이 존재한다.
- 이렇게 사용하면 **variable localVariable is already defined in method anotherMethod()** 라는 에러가 발생한다.
  + 이미 해당 메서드 내에서 선언된 변수를 다시 선언하려고 해서 발생함.
   
## 자바의 자료형

- 자바의 자료형은 아래와 같이 두 가지로 나뉜다.
  - 기본 자료형
  - 참조 자료형
 
- 두 자료형의 차이는 new를 사용해서 초기화를 하느냐, 없이 바로 초기화를 할 수있냐로 나뉜다.
- 기본 자료형 외의 모든 자료형을 참조 자료형이라고도 한다.


```java
// 기본 자료형
int a = 10; 

// 참조 자료형
Caculator calc = new Caculator();

```

![type](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FwRWZo%2Fbtr0eUsXBB6%2FjaWBWVxBGRFof4i1ZyfQ9k%2Fimg.png)


- 기본 자료형
  - 정수형: byte, short, int, long, char
  - 소수형: float, double
  - 진위형 : boolean

![t_size](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FldOSu%2FbtqJWrUhi14%2F5KT8c5swMVlRv5qOrrazf0%2Fimg.jpg)


**정수형**

- byte 
  - byte type의 최소값에서 1을 빼고 최대값에서 1을 더하면 각각 최대값, 최소값으로 반전된다.

 
```java
  
public class PrimitiveTypes{
     
    public static void main(String[] args){
        PrimitiveTypes type = new PrimitiveTypes();
        type.checkByte();

    }

    public void checkByte(){
        byte byteMin = -128;
        byte byteMax = 127;
        System.out.println("byteMin=" + byteMin);
        System.out.println("byteMax=" + byteMax);
        byteMin = (byte)(byteMin-1);
        byteMax = (byte)(byteMax+1);
        System.out.println("byteMin-1 = " + byteMin);
        System.out.println("byteMax+1 = " + byteMax);
        
    }
  // 결과
  // byteMin=-128
  // byteMax=127
  // byteMin-1 = 127
  // byteMax+1 = -128
}


```

- 각 정수형 타입들에 Overflow를 발생시켜보자
- 최대값에서 1을 더하면 음수 값으로 변경되는 것을 알 수있다.

```java

  public void checkAnotherType() {
    short shortMax = 32767;
    int intMax = 2147483647;
    long longMax = 9223372036854775807L;

    System.out.println(shortMax);
    System.out.println(intMax);
    System.out.println(longMax);

  
    shortMax = (short) (shortMax + 1); // This will cause overflow
    intMax = intMax + 1;               // This will cause overflow
    longMax = longMax + 1;             // This will cause overflow
    
    System.out.println("shortMax + 1 = " + shortMax);
    System.out.println("intMax + 1 = " + intMax);
    System.out.println("longMax + 1 = " + longMax);
}

32767
2147483647
9223372036854775807
shortMax + 1 = -32768
intMax + 1 = -2147483648
longMax + 1 = -9223372036854775808



```


**소수형**

소수점 값을 처리하기 위해 사용되어진다.<br>
  - float 과 double이 존재한다.
  - float은 32비트이며 double은 64비트이다.


**char/boolean**

char 
- 문자형, 2byte의 문자 하나를 입력하는 데이터형이다.
- 문자 1개를 저장하는 데이터형이라고 아는것이 중요
- char 변수명 = '값';

```java

    public static void main(String[] args)
    {
            char a1 = 'a';  // 문자로 표현
            char a2 = 97;  // 아스키코드로 표현
            char a3 = '\u0061';  // 유니코드로 표현

            System.out.println(a1);  // a 출력
            System.out.println(a2);  // a 출력
            System.out.println(a3);  // a 출력
    }


```

boolean
- 논리형, 참과 거짓을 저장하는 타입,  boolean의 기본값은 false
- 주로 yes/no, on/off 등의 논리 구현에 주로 사용되며 두가지 값만 표현하므로 가장 크기가 작다.
- boolean은 실제로 1bit면 충분하지만, 데이터를 다루는 최소 단위가 1byte이므로 메모리 크기가 1byte이다.


**기본 자료형의 기본값**

자바의 모든 자료형은 값을 지정하지 않으면 기본값을 사용한다.
단 **지역변수로 기본자료형을 사용할 때는 반드시 값을 지정해줘야 한다.**
즉 메서드 안에서 정의한 변수에 값을 지정하지 않고 사용하려고 하면 컴파일도 되지 않는다.
인스턴스 변수, 클래스 변수, 매개 변수는 값을 지정하지 않고도 컴파일이 되긴하지만 이렇게 값을 지정하지 않고 개발하는 것은 매우 안좋은 습관이다.

```java
public class TestType
{
    int intDefault;

    public void defaultValues(){
        int intDefault2;
        System.out.println(intDefault);
        System.out.println(intDefault2);
    }
}

-------------------------------------------------

public class main
{
   
    public static void main(String[] args)
    {
        TestType type = new TestType();
        type.defaultValue();
    }
}
// 에러 발생
// variable intDefault2 might not have been initialized 

```

- 지역 변수를 만들어 놓고 사용하지 않을 때에는 초기화를 하지 않아도 되지만, 사용할 때에는 반드시 초기화를 해야한다.
  
## 클래스 인스턴스 변수의 기본값은?

- 그렇다면 초기화를 하지 않았을때의 값은 어떻게 나올까

```java
  
public class PrimitiveType{
     int intDefault;
     byte byteDefault;
     short shortDefault;
     long longDefault;
     float floatDefault;
     double doubleDefault;
     char charDefault;
     boolean booleanDefault;

    public static void main(String[] args)
    {
        PrimitiveType type = new PrimitiveType();
        type.defaultValue();
    }


    public void defaultValue(){
        System.out.println("intDefault " + intDefault);
        System.out.println("byteDefault " + byteDefault);
        System.out.println("shortDefault " + shortDefault);
        System.out.println("longDefault " + longDefault);
        System.out.println("floatDefault " + floatDefault);
        System.out.println("doubleDefault " + doubleDefault);
        System.out.println("charDefault " + charDefault);
        System.out.println("booleanDefault " + booleanDefault);
    }

}

// 결과값
intDefault 0
byteDefault 0
shortDefault 0
longDefault 0
floatDefault 0.0
doubleDefault 0.0
charDefault 
booleanDefault = false

  ```
- char를 제외한 모든 숫자의 기본값은 0
- boolean의 기본값은 false



