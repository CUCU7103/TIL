# 변수

변수에는 4가지 종류가 존재합니다.
- 지역변수
  - 중괄호 내에서 선언된 변수
  - 지역변수를 선언한 중롹로 내에서만 유효하다.
 
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
















