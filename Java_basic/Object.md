# Object 

## 자바에서 모든 클래스의 최상위 부모 클래스는 항상 Object 입니다.

모든 클래스에서 Object 클래스를 상속받는 이유는 2가지로 볼 수있습니다.

1. **공통 기능제공**
2. 다형성의 기본구현

공통적인 기능을 제공한다고 하면 크게 3가지로 볼 수있다.
객체의 정보를 제공하고 , 객체가 다른 객체와 같은지 비교하고, 객체가 어떤 클래스로 만들어 졌는지 이러한 기능은 모든 클래스에
반드시 공통적으로 존재해야하는 기능이다.

즉 Object는 모든 객체에 필요한 공통 기능을 제공하는 부모 클래스로써 자식 클래스들이 해당 기능을 상속받아서 편리하게 사용할 수 있도록 해준다.
1. 객체의 정보를 제공하는 toString()
2. 객체의 같음을 비교하는 equals()
3. 객체의 클래스 정보를 제공하는 getClass()
4. 객체가 생성되어질때 임의의 변하지 않는 정수값을 제공하는 hashCode() (런타임 중에만, 어플리케이션 종료후에는 해당 값이 변할 수있다.)
5. 기타 여러가지 기능등...

또한 Object는 모든 클래스의 부모 클래스가 되므로써 모든 객체를 참조할 수있다.
즉 타입이 다른 객체들을 Object 객체에 보관할 수 있다는 뜻이다.

```
public class ObjectPolyExampleEx1 {

    public static void main(String[] args) {
        Dog dog = new Dog();
        Car car = new Car();

        action(dog);
        action(car);
    }

    private static void action(Object obj) {
        //obj.sound(); //컴파일 오류, Object는 sound()가 없다.
        //obj.move(); //컴파일 오류, Object는 move()가 없다.

        //객체에 맞는 다운캐스팅 필요
        if (obj instanceof Dog dog) { 
            dog.sound();
        } else if (obj instanceof Car car) {
            car.move();
        }
    }
}

```

### 1. toString()

- 객체의 정보를 문자열 형태로 제공합니다.
- 디버깅과 ,로깅에 유용하게 사용되어집니다.
- 좀 더 객체의 자세한 정보를 제공하기 위해서 오버라이딩해서 사용한다.
- System.out.println() 안에서 toString()을 호출한다.

```
public class ObjectPrinter {

    public static void print(Object obj) {
        String string = "객체 정보 출력: " + obj.toString();
        System.out.println(string);
    }
}

public class Dog {
    private String dogName;

    public Dog(String dogName) {
        this.dogName = dogName;
    }

    @Override
    public String toString() {
        return "Dog{" +
                "dogName='" + dogName + '\'' +
                '}';
    }
}


public class Car {
    private String carName;

    public Car(String carName) {
        this.carName = carName;
    }
}

-------------------------------------------------------------------------
public class ToStringMain2 {
    public static void main(String[] args) {
        Car car = new Car("k9");
        Dog dog = new Dog("Blue");


        System.out.println("단순 toString() = ");
        System.out.println(car.toString());
        System.out.println(dog.toString());

        System.out.println("println 내부에서 toString()호출");
        System.out.println(car);
        System.out.println(dog);

        System.out.println("Object 다형성 활용");
        ObjectPrinter.print(car);
        ObjectPrinter.print(dog);

    }
}

// 결과
단순 toString() = 
object.toString.Car@41629346
Dog{dogName='Blue'}

println 내부에서 toString()호출
object.toString.Car@41629346
Dog{dogName='Blue'}

Object 다형성 활용
객체 정보 출력: object.toString.Car@41629346
객체 정보 출력: Dog{dogName='Blue'}
```




