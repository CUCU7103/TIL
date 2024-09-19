## 클래스 
클래스는 자바에서 가장 작은 단위이며,
클래스는 상태(속성) 행동(기능)을 가지고 있어야 합니다. <br>
보통 위 두개를 가지고 있어야만 한다고 말하지만, 없다고 클래스를 만들 수 없는 것은 아니다

클래스를 한번 만들어보자.

```java
public class Car {
  // 상태
  int speed;  // 자동차의 주행 중인 상태
  int distinct; // 자동차의 주행한 상태
  String color; // 자동차의 색깔의 상태

// 생성자
public Car(){
}

// 메서드
// 클래스의 상태를 변경하는 행위를 지정함.  
public void speedUp(){ // 속도를 증가시키는 행위
   speed = speed+5;
}

public void breakDown(){ // 속도를 감소시키는 행위
  speed = speed-10;
}

public int getCurrentSpeed(){
  return speed;
}

}
```

## 클래스와 객체

일반적으로 클래스를 붕어빵틀, 객체를 붕어빵이라고 비교한다.
붕어빵틀을 통해서 여러 종류, 혹은 많은 붕어빵을 찍어낼 수 있다.
그리고 **클래스를 사용하기 위해서는 객체를 생성해야한다.**
추가적으로 **클래스를 기반으로 실제 메모리에 만들어진 실체를 객체 혹은 인스턴스**라고 칭한다.

```java
public CarManager{

// 2개의 객체를 생성하였다.
Car car1 = new Car();
Car car2 = new Car();

}
```
여기서 Car() 라는 기본생성자를 사용하는것을 볼 수 있다.<br>

**생성자는 객체를 생성하는 거의 유일한 도구이다.**
<br>

기본생성자는 클래스 내에서 선언하지 않아도 클래스를 컴파일 때 자동으로 생성되어진다.

## new 
new는 자바에서 미리 정해져 있는 예약어중 하나이다.<br>
  - new 뒤에다가 생성자를 지정하면 된다.
  - new 라는 예약어를 통해서 생성자인 Car()를 호출하면 위의 코드처럼 car1, car2 객체가 사용되어진다.

클래스는 대부분 객체를 생성해야만 일을 시킬 수 있다.<br>
car1에 대해서 속도를 높이거나 색을 바꾸거나 등의 행위를 하고자 한다면<br>
아래와 같이 코드를 작성하면 된다. <br>
```java
Car car1 = new Car();
car1.speedup();
car1.breakDown();
```

전체적으로 구현한 코드를 확인해보자 <br>

**Car class**
```java

public class Car {
  // 상태
  int speed;  // 자동차의 주행 중인 상태
  int distinct; // 자동차의 주행한 상태
  String color; // 자동차의 색깔의 상태

// 생성자
public Car(){
}

// 메서드
// 클래스의 상태를 변경하는 행위를 지정함.  
public void speedUp(){ // 속도를 증가시키는 행위
   speed = speed+5;
}

public void breakDown(){ // 속도를 감소시키는 행위
  speed = speed-10;
}

public String changeColor(String changeColor){
    this.color = changeColor;
    return color;
}

public int getCurrentSpeed(){
  return speed;
}

public String getColor(){
    return color;
}
}

```
<br>

**실행할 main 클래스**

```java

public class main{
      public static void main(String[] args) {
        Car car = new Car();
        car.speedUp();
        car.speedUp();
        car.changeColor("blue");
        System.out.println(car.getCurrentSpeed());
        System.out.println(car.getColor());
        // 결과 : 10
        //        blue
    }
    
}

```













