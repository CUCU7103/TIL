# 반복문

- 반복문은 말 그대로 특정 코드를 반복해서 실행할 때 사용되어집니다.

```java
public class ControlofFlow {
  public static void main(String args[]) {
        ControlofFlow sample = new ControlofFlow();
        sample.switchStatement(1);
        sample.switchStatement(2);
  }
  
  public void switchStatement(int month) {
      switch(month) {
          case 1: 
          case 3:
          case 5:
          case 7:
          case 8:
          case 10:
          case 12:
           System.out.println(month + "has 31 days");
           break;
          case 4: 
          case 6:
          case 9:      
          case 11:
           System.out.println(month + " has 30 days");
           break;
          case 2:
            System.out.println(month + " has 28 or 29 days. ");
            break;
          default:
           System.out.println(month + " has 28 or 29 days. ");
            break;
      }
  }
}
```
위의 코드에서 반복문을 사용하여 모든 월을 출력하려고 하면

```java
public class ControlWhile {
  public static void main(String args[]) {
        ControlWhile sample2 = new ControlWhile();
        sample2.whileLoop1();
  }
  public void whileLoop1(){
        ControlSwitch sample = new ControlSwitch();
        int loop = 0;
        while(loop < 12){
            loop++;
            sample.switchCalendar(loop);
        }
   }
}

```
이런식으로 구현할 수 잇다.

## break

여기서 while문을 사용하는데 특정한 월 이상을 출력하고 싶지 않다고 하면<br>
while(조건식) 이 참일때만 동작하므로

```java
    public void whileLoop2(){
        ControlSwitch sample = new ControlSwitch();
        int loop = 0;
        while(loop < 12){
            loop++;
            sample.switchCalendar(loop);
            if(loop == 7) loop = 20;
        }
        
   }


   public void whileLoop3(){
        ControlSwitch sample = new ControlSwitch();
        int loop = 0;
        while(loop < 12){
            loop++;
            sample.switchCalendar(loop);
            if(loop == 7) break;
        }

```
이런 식으로 조건이 거짓이 되게 해서 종료시키거나

break문을 사용하면된다.

## continue

- 반복문에서 continue를 만나게 되면 그 이후에 있는 문장들은 수행되지 않고 바로 반복문의 조건을 점검하는 곳으로 이동한다.

```java

  public void whileContiune(){
       ControlSwitch sample = new ControlSwitch();
       int loop = 0;
       while(loop < 12){
           loop++;
           if(loop == 6 ) continue;
           sample.switchCalendar(loop);
       }
   }

//결과
1has 31 days
2 has 28 or 29 days. 
3has 31 days
4 has 30 days
5has 31 days
7has 31 days
8has 31 days
9 has 30 days
10has 31 days
11 has 30 days
12has 31 days

```

단 무한반복되는 것을 조심해야한다.

## do-while

- 적어도 한 번이상의 반복문이 실행되어진다.
- 반드시 한번을 꼭 실행 시키고 싶을때 사용되어짐.

```
 do {
  처리문장;

  } while(boolean 조건);
```
<br>

```java
  public class DoWhile2 {

    public static void main(String[] args) { 
        int i = 10;
        do { 
            System.out.println("현재 숫자는:" + i);
            i++;
        } while (i < 3);
    } 
}
```

## for 문

```java

for (1.초기식; 2.조건식; 4.증감식) {
 // 3.코드 }

```
for문은 다음 순서대로 실행된다.
1. 초기식이 실행된다. 주로 반복 횟수와 관련된 변수를 선언하고 초기화 할 때 사용한다. 초기식은 딱 1 번 사용된다.
2. 조건식을 검증한다. 참이면 코드를 실행하고, 거짓이면 for문을 빠져나간다.
3. 코드를 실행한다.
4. 코드가 종료되면 증감식을 실행한다. 주로 초기식에 넣은 반복 횟수와 관련된 변수의 값을 증가할 때사용한다.
5. 다시 2. 조건식 부터 시작한다. (무한 반복)



- 예시

```java
public class For1 {
public static void main(String[] args) {
  for (int i = 1; i <= 10; i++) {
       System.out.println(i);
    }
  }
}
```
1. 초기식이 실행된다. `int i = 1`
2. 조건식을 검증한다. `i <= 10`
3. 조건식이 참이면 코드를 실행한다. `System.out.println(i);`
4. 코드가 종료되면 증감식을 실행한다. `i++`
5. 다시 2. 조건식을 검증한다. (무한 반복) 이후 `i <= 10` 조건이 거짓이 되면 for문을 빠져나간다.

## 중첩 반복문

```java

public class Nested1 {

public static void main(String[] args) {
    for (int i = 0; i < 2; i++) {
              System.out.println("외부 for 시작 i:" + i);
        for (int j = 0; j < 3; j++) {
            System.out.println("-> 내부 for " + i + "-" + j);
        }
        System.out.println("외부 for 종료 i:" + i);
        System.out.println(); //라인 구분을 위해 실행
    }
  }
}

// 결과
외부 for 시작 i:0
-> 내부 for 0-0
-> 내부 for 0-1
-> 내부 for 0-2
외부 for 종료 i:0

외부 for 시작 i:1
-> 내부 for 1-0
-> 내부 for 1-1
-> 내부 for 1-2
외부 for 종료 i:1

```











