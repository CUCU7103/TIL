# 조건문 IF
## IF
- if문의 소괄호 뒤에는 기본적으로 어떤 문장이 있든 간에 세미콜론이 하나만 있으면 컴파일/실행이 정상적으로 된다.
<p>
  
```java
public class ControlIf {
  public static void main(String args[]) {
    ControlIf sample = new ControlIf();
    sample.ifStatement();
  } 
  
  
  public void ifStatement(){
      if(true);
      if(true) System.out.println("It's true");
      if(true) 
        System.out.println("It's also true");
    
    if(false)System.out.println("It's false");
  }
}

```

- if(참) 일때만 뒤의 문장이 실행되어진다.
- 즉 if(boolean 값)의 상태에 따라서 실행되어질지 아닌지 결정되어진다.

## IF/else 

```
  if(boolean) 처리문장;
  else 처리문장2;
```
- 예시

``` java

public class ControlIfElse {
  public static void main(String args[]) {
    ControlIfElse sample = new ControlIfElse();

    sample.ifElseStatement();
  } 
  
  public void ifElseStatement(){
      
      int intValue = 10;
      
      if(intValue > 5) System.out.println("It's bigger than 5");
      else System.out.println("It's smaller or equal than 5.");
      
      if(intValue <= 5)
        System.out.println("It's smaller equal than 5");
      else
        System.out.println("It's bigger than 5.");
  }
  
}

```

###

- if 다음에는 문장이 여러개 올 수있다.
```
if(booelean 값) ..... 

if(booelean 값){

 문장1 ...;
 문장2 ...;

}

---------------------------

if(booelean 값){
  문장1 ...;
  문장2 ...;

}else{
  문장1 ...;
  문장2 ...;

}

```

- if의 조건은 여러개를 사용할 수 있다.
- 2개 이상의 조건이면 괄호를 사용해서 묶어주는게 가독성이 좋아진다.
``` java
  
public class ControlIfAndOr {
  public static void main(String args[]) {
    ControlIfAndOr sample = new ControlIfAndOr();
    sample.ifAndOr();
    sample.tripleOrAnd();
  } 
  
  public void ifAndOr(){
      
      int age =25;
      boolean isMarried = true;
      
      if(age > 25 && isMarried){ // 두 조건 모두 참이여야 한다.
          System.out.println("Age is over 25 and Married");
      }
      
      if(age > 25 || isMarried){ // 둘 중에 하나만 참이면 된다.
           System.out.println("Age is over 25 or Married");
      }
      
  }


  public void tripleOrAnd(){
      int age = 25;
      boolean isMarried = true;
      double height = 180;
      
      if(age > 25 || isMarried && height >= 180){
          System.out.println("Age is over 25 or Married and height and is over 180");
      }
      
  }

  
}
// Age is over 25 or Married
// Age is over 25 or Married and height and is over 180

```

- if/else를 잘 사용하면 효율적으로 조건을 따질 수 있다.

```java

public class ControlElseIf {
  public static void main(String args[]) {
    ControlElseIf sample = new ControlElseIf();
    sample.elseIf();
  } 

  
  public void elseIf(){
  
    int point = 74;
  
    if(point > 90){
        System.out.println("A");
    }else if(point > 80){
        System.out.println("B");
    }else if(point > 70){
        System.out.println("C");
    }else if(point > 60){
        System.out.println("D");
    }else{
        System.out.println("F");
    }
      
  }
  
  
}
// C
```


# SWITCH

- if else는 두가지 이상의 값을 비교하거나 단순히 true, false 여부만 확인하고자 할때 많이 사용한다.
- 하나의 값을 여러분기로 비교해야 될 때는 switch 구문을 사용하는 것이 좋다.

```java

switch (조건식) { case value1:
// 조건식의 결과 값이 value1일 때 실행되는 코드 break;
case value2:
// 조건식의 결과 값이 value2일 때 실행되는 코드 break;
default:
// 조건식의 결과 값이 위의 어떤 값에도 해당하지 않을 때 실행되는 코드
}

```

- 조건식의 결과값이 어떤 case와 일치하면 해당 case의 코드를 실행한다.
- break문은 현재 실행 중인 코드를 끝내고 switch문을 빠져나가게 하는 역할을 한다.
- break문이 없으면 일치하는 case 이후의 모든 case 코드들이 순서대로 실행된다.
- default는 조건식의 결과값이 모든 case의 값과 일치하지 않을때 실행된다.
  - if문의 else와 같다.
- if,else-if,else 구조와 동일하다.

```java

public class ControlSwitch {
  public static void main(String args[]) {
        ControlSwitch sample = new ControlSwitch();
        sample.switchStatement(1);
        sample.switchStatement(2);
        sample.switchStatement(3);
        sample.switchStatement(4);
        sample.switchStatement(5);
  }
  
  public void switchStatement(int numberOfwheel) {
      switch(numberOfwheel) {
          case 1: 
              System.out.println(numberOfwheel + ": It is a one-wheel bicycle");
              break;
          case 2: 
              System.out.println(numberOfwheel + ": It is a motorcycle or bicycle");
              break;
          case 3: 
              System.out.println(numberOfwheel + ": It is a three-wheel car");
              break;
          case 4: 
              System.out.println(numberOfwheel + ": It is a car");
              break;
          default:
              System.out.println(numberOfwheel + ": It is an expensive car");
              break;
      }
  }
}


```











