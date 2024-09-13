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

















