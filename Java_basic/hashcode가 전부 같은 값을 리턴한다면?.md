# hashCode가 전부 같은 값을 리턴한다면??

유형에 따라서 조금 다른 문제가 발생합니다

1. 일반적인 참조객체 (Reference type)
- 일반적인 객체에서는 동등성을 비교할때 기본적으로 equals()만을 사용합니다. hashCode()는 크게 중요하지 않습니다.
- 계약 위반:
  - hashCode()와 equals()는 특정한 약속이 있습니다 equals() 가 true면 반드시 같은 hashCode()를 가져야합니다.
- 단 다른 객체라고 해서 무조건 hashCode() 값이 다르지는 않습니다.
  - String의 경우 다른 객체여도 hashCode() 값이 다를 수 있습니다.

2. 해시 기반 자료구조
 - 동일한 hash 자료구조를 가질 때 가장 큰 문제가 발생합니다.
 - 해당 자료구조에서는 hashCode()를 기반으로 데이터를 저장하고 검색하는 것이 핵심입니다.
 - 동등성 비교를 할때 먼저 hashCode()를 검사한뒤 equal()를 통해 검사합니다.
![image](https://github.com/user-attachments/assets/c26b46ed-82c1-4c3e-933d-1dd0b069f616)

```
class Person {
    String name;
    int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Person person = (Person) o;
        return age == person.age && name.equals(person.name);
    }

    // 해시 충돌을 피하는 경우: 고유한 hashCode를 반환
    @Override
    public int hashCode() {
        return name.hashCode() + age;
    }
}

class PersonWithCollision {
    String name;
    int age;

    public PersonWithCollision(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        PersonWithCollision person = (PersonWithCollision) o;
        return age == person.age && name.equals(person.name);
    }

    // 해시 충돌을 유발하는 경우: 모든 객체가 동일한 hashCode를 반환
    @Override
    public int hashCode() {
        return 1; // 모든 객체의 hashCode가 동일
    }
}
//=================================================================================================================
 class HashMapPerformanceTest {
    public static void main(String[] args) {
        int numberOfEntries = 100_000;

        // 해시 충돌이 없는 경우 테스트
        HashMap<Person, String> noCollisionMap = new HashMap<>();
        long startTime = System.nanoTime();
        for (int i = 0; i < numberOfEntries; i++) {
            noCollisionMap.put(new Person("Person" + i, i), "Value" + i);
        }
        long endTime = System.nanoTime();
        System.out.println("해시 충돌이 없는 경우 삽입 시간: " + (endTime - startTime) / 1_000_000.0 + " ms");

        startTime = System.nanoTime();
        for (int i = 0; i < numberOfEntries; i++) {
            noCollisionMap.get(new Person("Person" + i, i));
        }
        endTime = System.nanoTime();
        System.out.println("해시 충돌이 없는 경우 검색 시간: " + (endTime - startTime) / 1_000_000.0 + " ms");

        // 해시 충돌이 있는 경우 테스트
        HashMap<PersonWithCollision, String> collisionMap = new HashMap<>();
        startTime = System.nanoTime();
        for (int i = 0; i < numberOfEntries; i++) {
            collisionMap.put(new PersonWithCollision("Person" + i, i), "Value" + i);
        }
        endTime = System.nanoTime();
        System.out.println("해시 충돌이 있는 경우 삽입 시간: " + (endTime - startTime) / 1_000_000.0 + " ms");

        startTime = System.nanoTime();
        for (int i = 0; i < numberOfEntries; i++) {
            collisionMap.get(new PersonWithCollision("Person" + i, i));
        }
        endTime = System.nanoTime();
        System.out.println("해시 충돌이 있는 경우 검색 시간: " + (endTime - startTime) / 1_000_000.0 + " ms");
    }
}
//
해시 충돌이 없는 경우 삽입 시간: 42.7822 ms
해시 충돌이 없는 경우 검색 시간: 21.1989 ms
해시 충돌이 있는 경우 삽입 시간: 128746.6547 ms
해시 충돌이 있는 경우 검색 시간: 121340.6093 ms
```

- **HashMap**에서 모든 객체의 hashCode()가 동일하면, 성능이 O(1)에서 O(n)으로 떨어지며,
-  해시 충돌이 자주 발생하여 equals()가 반복적으로 호출됩니다. 이는 데이터가 많아질수록 성능 저하로 이어집니다.
- **HashSet**에서도 hashCode()가 동일하면 중복 처리가 equals()에 의존하게 되고, 해시 충돌로 인해 성능이 저하됩니다.


3. hash 기반이 아닌 자료구조
   - hashCode()의 영향을 받지 않기 때문에 큰 문제는 발생하지 않습니다.











