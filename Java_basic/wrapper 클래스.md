# Java.lang

### 자바의 lang 패키지는 자바 프로그래밍에 필요한 가장 기본적인 클래스들이 모여있는 패키지 입니다.

- import 구문으로 호출할 필요없이 바로 사용이 가능합니다.
  
![image](https://github.com/user-attachments/assets/d109953d-24a7-450b-bb1f-776af3c38591)

몇가지 대표적인 클래스들을 살펴보겠습니다.

1. Wrapper 클래스
   - 자바에서는 8개의 기본자료형(Primitive tyupe)을 사용합니다, 그런데 이러한 기본자료형을 객체로 취급해야 할 경우가 있습니다
   - 이때 **8개의 기본 타입에 해당하는 데이터를 객체로 포장해주는 클래스를 래퍼 클래스라고 합니다.**

![image](https://github.com/user-attachments/assets/897dcc76-e189-4f04-884f-e0fa18c49415)

   - 기본 타입의 데이터를 래퍼 클래스의 인스턴스로 변경하는 것을 박싱(boxing)이라고 하며
   - 래퍼 클래스의 인스턴스에 저장된 값을 다시 기본 타입의 데이터로 꺼내는 과정을 언박싱이라고 합니다.

![image](https://github.com/user-attachments/assets/7d47eeec-1f8e-4169-9159-1a0b67c582c8)

```
// 박싱
Integer num = new Integer(20); // Integer 래퍼 클래스 num 에 21 의 값을 저장

// 언박싱 (intValue)
int n = num.intValue(); // 래퍼 클래스 num 의 값을 꺼내 가져온다.

// 재 포장(박싱)
n = n + 100; // 120
num = new Integer(n);

```

- 이러한 박싱과 언박싱을 자바 컴파일러가 자동으로 처리해주기 시작했습니다.

```
/* 기존 박싱 & 언박싱 */
Integer num = new Integer(17); // 박싱
int n = num.intValue();        // 언박싱

/* 오토 박싱 & 언박싱 */
Integer num = 17; // new Integer() 생략
int n = num; // intValue() 생략

```

- 이에 따라서 wrapper 클래스의 연산이 좀 더 간단해졌습니다.
- 객체간의 직접적인 연산은 불가능하지만 오토 언박싱과 오토박싱을 통해서 직접적인 연산이 가능해졌습니다.
``
public class wrapperEx1 {

    public static void main(String[] args) {
        Integer num1 = new Integer(7);
        Integer num2 = new Integer(3);

        int int1 = num1.intValue();
        int int2 = num2.intValue();
        //=============================================

        Integer result1 = num1 + num2;
        Integer result2 = int1 = int2;
        int result3 = num1 * int2;
        System.out.println(result1);
        System.out.println(result2);
        System.out.println(result3);
    }
}
```

- 래퍼 클래스 간의 동등 비교는 객체간의 동등 비교이기 때문에 == 이 아닌 equals()를 사용해야 합니다.
- 단 래퍼 클래스와 기본자료형과의 비교는 오토박싱과 언박싱이 진행되기 때문에 == 과 equals() 사용이 가능합니다.


- 객체를 포장하는 기능 외에도, 래퍼 클래스는 자체 지원하는 parse() 메소드를 이용하여 데이터 타입을 형 변환 할때도 유용하게 사용되어집니다.

```
String str = "10";
String str2 = "10.5";
String str3 = "true";

byte b = Byte.parseByte(str);
int i = Integer.parseInt(str);
short s = Short.parseShort(str);
long l = Long.parseLong(str);
float f = Float.parseFloat(str2);
double d = Double.parseDouble(str2);
boolean bool = Boolean.parseBoolean(str3);

System.out.println("문자열 byte값 변환 : "+b);
System.out.println("문자열 int값 변환 : "+i);
System.out.println("문자열 short값 변환 : "+s);
System.out.println("문자열 long값 변환 : "+l);
System.out.println("문자열 float값 변환 : "+f);
System.out.println("문자열 double값 변환 : "+d);
System.out.println("문자열 boolean값 변환 : "+bool);
```



