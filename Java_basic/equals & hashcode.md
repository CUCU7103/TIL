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
**어떤 클래스는 사람의 이름으로 동등성을 처리할 수있고, 어떤 클래스는 주민번호를 기준으로 동등성을 처리할 수잇다.**

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




