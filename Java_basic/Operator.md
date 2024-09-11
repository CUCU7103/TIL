## 연산자

### 1. 기본적인 연산자

  - 대입 연산자 **=**
    - 변수에 값을 대입할 때 사용하는 연산자이다.

  - 산술 연산자
    1. "+" : 더하기 연산자
    2. "-" : 빼기 연산자
    3. "*" : 곱하기 연산자
    4. "/" : 나누기 연산자
    5. "%" : 나머지 연산자

**"+ , -"**
```java

public class OperatorPlusMinus {
  public static void main(String args[]) {
    OperatorPlusMinus sample = new OperatorPlusMinus();
    sample.additive();
  }
 
 
 public void additive(){
     int intValue1 = 5; 
     int intValue2 = 10;
 
     int result = intValue1 + intValue2 ;
     System.out.println(result);
     result = intValue2 - intValue1;
     System.out.println(result);
 }
// 결과
 // 15
 // 5
}
```

**" * ,/ "**

```java

public class OperatorMutipleDivision {
  public static void main(String args[]) {
    OperatorMutipleDivision sample = new OperatorMutipleDivision();
    sample.mutipleDivision();
  }
 
 public void mutipleDivision(){
     int intValue1 = 5;
     int intValue2 = 10;
     
     int result = intValue1 * intValue2;
     System.out.println(result);
     result = intValue2 / intValue1;
      System.out.println(result);
     
 }
 // 결과
// 50
// 2 
}

// 만일 result에서 intValue1 / intValue2 를 하면?
// 0.5가 나와야되지만 int는 정수형만 표현 할 수 있기 때문에 0 이됨
// double 이나 float를 사용해서 연산해야 된다.

```

**" % "**
```java

public class OperatorReminder {
  public static void main(String args[]) {
    OperatorReminder sample = new OperatorReminder();
    sample.reminder();
  }
 
 public void reminder(){
     int intValue1 = 53;
     int intValue2 = 10;
     
     int result = intValue1 % intValue2;
     System.out.println(result);
     
 }

// 결과 3
// 나머지 값을 구하는 연산자이다.
}
```

### 2. 복합 대입 연산자

```java

int intValue = 10;
intvValue = intValue + 5

// 위와 같은 계산을 아래와 같이 변경할 수 있다.
// intValue에 5를 더한것을 intValue에 할당하라.
intValue += 5;



```

- 복합대입연산자 

![image](https://github.com/user-attachments/assets/d7f1e9a4-dfe7-4ba1-a93d-e3a0640727d2)


```java

 public void compond(){
    int intValue = 10;
    intValue += 5;
    System.out.println(intValue);
    intValue -= 5;
    System.out.println(intValue);
    intValue *= 5;
    System.out.println(intValue);
    intValue /= 5;
    System.out.println(intValue);
    intValue %= 5;
    System.out.println(intValue);

}

// 결과
15
10
50
10
0

```

### 3. 단항연산자

1. 부호 연산자
   
    ![image](https://github.com/user-attachments/assets/cfaf8108-6679-41b0-b14c-a81db58b311c)

2. 증감 연산자
   - "++" , "--" 

```java

  int intValue = 10
  intValue = intValue + 1;
  intValue = intValue - 2;

// 위 코드를 아래와 같이 변경할 수 있다.
  intValue++;
  intValue--;

```
 - 증감 연산자는 변수의 앞이나 뒤에 붙을 수 있다.
   
```java

public class OperatorIncrement {
  public static void main(String args[]) {
    OperatorIncrement sample = new OperatorIncrement();
    //sample.reminder();
    sample.increment();
  }
 
    public void increment(){
        int value = 1;
        System.out.println(value++); // 변수를 참조한 후에 1 증가
        System.out.println(value); 
        System.out.println(++value); // 변수를 참조하기 전에 1 증가
    }

}

```


3. 부정 연산자 

**" ! "**
  - boolean type에서만 이 연산자를 사용한다.

![image](https://github.com/user-attachments/assets/4b9ffe45-38c5-493b-b4cb-9099b15416ca)


### 계산하는 순서

  1. 단항연산자 (++,--,+,-,! ~)
  2. 산술연산자 ( *,/,%)
  3. 산술연산자 (+,-)


4. 비교 연산자

  - 모든 비교 연산자의 결과는 반드시 boolean 형이여야 한다.
  
   
![image](https://github.com/user-attachments/assets/9629a8e0-3598-47b1-93c5-cec6ab1ed4cf)











       
