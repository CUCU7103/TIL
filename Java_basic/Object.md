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

# equals & hashcode 

## equals 

equals에 대해서 알아보려면 먼저 동등성과 동일성에 대해서 알아봐야한다.

자바는 두 객체가 같다는 걸 2가지로 표현합니다.

- 동일성
  - == 연산자를 사용해서 **두 객체의 참조가 동일한 객체를 가리키고 있는지 확인한다.**
  - 즉 물리적으로 같은 메모리에 존재하는 객체 인스턴스 인지 참조값을 확인하는 것입니다.
    
- 동등성 :
  - equals() 메서드를 사용하여 **두 객체가 논리적으로 동등한지 확인**
  - 보통 사람이 생각하는 논리적인 기준에 맞추어 비교합니다.

그런데 아래와 같이 객체를 비교하는데 ==, equals()를 사용했는데 동일한 결과가 나왔다 왜 그럴까?

  ```
  public class EqualsMain1 {

    public static void main(String[] args) {
        UserV1 user1 = new UserV1("id-100");
        UserV1 user2 = new UserV1("id-100");

        System.out.println("identity = " + (user1 == user2));
        System.out.println("equality = " + user1.equals(user2));
    }
}
  ```

왜나하면  equals()는 기본적으로 아래와 같이 구성되어져 있다.
```
Object.java...

public boolean equals(Object obj) {
        return (this == obj);
    }
```

Object가 기본적으로 제공하는 equals()는 == 으로 동일성 비교를 제공한다.
잘 생각해보면 **동등성이라는 개념은 각각의 클래스마다 다르다.**
**어떤 클래스는 사람의 이름으로 동등성을 처리할 수있고, 어떤 클래스는 주민번호를 기준으로 동등성을 처리할 수았습니다.**

따라서 동등성 비교를 사용하려면 equals()를 오버라이딩 해서 사용해야한다.

equals의 오버라이딩은 일반적으로 IDE에서 지원해준다.

하지만 equals만 오버라이딩 하는 것은 문제가 있다.

## hashcode

- 객체의 고유한 주소 값을 해시 알고리즘을 사용하여 생성한 int 값입니다.
- 애플리케이션 실행되는 동안에는 객체의 hashCode()로 생성한 값은 고유합니다.
- 하지만 애플리케이션을 다시 시작한다면 이 값은 변경될 수 있습니다.


왜 equals()를 재정의 할때 hashCode()를 재정의 해야하는가 ?

1. 규약이 존재한다.
    - **동등한 객체는 동일한 해시 코드를 가져야 한다 즉, a.equals(b)가 true이면 a.hashCode() == b.hashCode()도 true여야 합니다. 라는 두 메서드 간의 규약이 있습니다.**
    - 이로 인해서 equals를 오버라이딩하면 hashCode 도 오버라이딩 해야 합니다.
    - equals()로 논리적으로 객체가 같다고 할 수있지만 그렇다고 그 객체의 주소값이 같다는 건 아니기에 hashCode()로 오버라이딩 해줘야한다.

2. hash 값을 사용하는 해시 기반 컬랙션을 사용할때 문제가 발생합니다.

아래의 코드를 보겠습니다.
```

public class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // equals() 오버라이딩 인텔리제이를 통한.
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;

        Person person = (Person) o;
        return age == person.age &&
                Objects.equals(name, person.name);
    }
}

==========================
public class PersonMain {

    public static void main(String[] args) {
        HashSet<Person> person = new HashSet<>();
        Person person1 = new Person("Joon",29);
        Person person2 = new Person("Joon",29);

        person.add(person1);
        person.add(person2);

        // 두 객체는 서로 동등한 관계이다. 즉 같은 객체라고 본다.
        // set은 중복되는 값을 받지 않는다.
        System.out.println("size = "+ person.size());


    }
}

결과 값 ==> 2

```
동등한 객체를 Set에 add()를 사용하여 넣어주엇는데 크기가 2가 나오는 문제가 발생합니다.


HashMap, HashSet 등과 같은 **해시 기반 컬렉션은 객체를 저장하거나 검색할 때 hashCode()를 사용합니다.**

해시 기반 컬렉션 은 객체가 논리적으로 같은지 비교할 때 아래 그림과 같은 과정을 거칩니다.

![image](https://github.com/user-attachments/assets/04fe1cb6-6276-4790-a165-65e732b24e17)

즉 hashCode()의 리턴 값이 우선 일치하고 equals 메서드의 리턴 값이 true여야 논리적으로 같은 객체라고 판단합니다.

Object 클래스의 hashCode 메서드는 객체의 고유한 주소 값을 int 값으로 변환하기 때문에 객체마다 다른 값을 리턴합니다.
두 개의 Person 객체는 equals로 비교도 하기 전에 서로 다른 hashCode 메서드의 리턴 값으로 인해 다른 객체로 판단되어서 해당 오류가 발생하는 것입니다.

해시코드를 오버라이딩 한뒤 실행시키면 동일한 값이 나옵니다.
```
public class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // hashCode 오버라이딩
    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }

    // equals() 오버라이딩
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;

        Person person = (Person) o;
        return age == person.age &&
                Objects.equals(name, person.name);
    }
}

.. 이하 생략
```
해시 컬랙션을 사용하지 않는 상황이라면 정의할 필요가 없지 않나 라고 생각할 수도 잇지만 자바의 규약과, 클래스를 사용하는 상황은 언제 변할지 모르기에
위험부담을 감수할 필요없이 사용하는 것이 좋을것 같습니다.


