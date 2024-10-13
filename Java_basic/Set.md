# Set 

![KakaoTalk_20241013_174834869_01](https://github.com/user-attachments/assets/62e59585-07b8-4fda-bb90-de4683e1ba20)


- set은 순서에 상관없이 어떤 데이터가 존재하는지를 확인하기 위한 용도로 많이 사용되어집니다.
- 즉 중복되는 것을 방지하고, 원하는 값이 포함되어있는지를 확인하는 것이 주 용도입니다.
- 어떤 값이 존재하는지 없는지 여부가 중요할때 set을 사용하면 됩니다.

#### Set 인터페이스를 구현한 주요 클래스는 다음과 같습니다

> [!IMPORTANT]
>
> ### Hashset :
>
> - 순서가 전혀 필요 없는 데이터를 해시 테이블에 저장합니다, set 중에 가장 성능이 좋습니다.
>
> ### TreeSet: 
>
> - 저장된 데이터의 값에 따라서 정렬되는 셋입니다. red-black 이라는 트리 타입으로 값이 저장되며 Hashset 보다 약간 성능이 느립니다.
>
> ### LinkedHastSet:
>
> - 연결된 목록 타입으로 구현된 해시 테이블에 데이터를 저장합니다.



### SET의 주요한 메서드

```
boolean add(E e)
주어진 객체를 저장 후 성공적이면 true를 중복 객체면 false를 리턴합니다.

boolean contains(Object o)
주어진 객체가 저장되어있는지 여부를 리턴합니다.

Iterator<E> iterator()
저장된 객체를 한번씩 가져오는 반복자를 리턴합니다.

isEmpty()
컬렉션이 비어있는지 조사합니다.

int Size()
저장되어 있는 전체 객체수를 리턴합니다.

void clear()
저장된 모든 객체를 삭제합니다.

boolean remove(Object o)
주어진 객체를 삭제합니다.

boolean removeAll(Collection c)	주어진 컬렉션에 저장된 모든 객체와 동일한 것들을 HashSet에서 삭제한다.(차집합)
(성공하면 true, 실패하면 false를 반환한다.)
```



##### 

## HashSet

HashSet의 생성자

```
HashSet() HashSet객체를 생성한다.
HashSet(Colleaction c)	주어진 컬렉션을 포함하는 HashSet객체를 생성한다.
HashSet(int initialCapacity) 주어진 값을 초기용량으로 하는 HashSet객체를 생성한다.
HashSet(int initialCapacity, float loadFactor)	초기용량과 load factor를 지정하는 생성자.
```

load factor란 ?

- 데이터의 갯수 / 저장공간을 의미합니다.  

- 헤시 테이블에 저장된 데이터 갯수 n을 버킷의 갯수 k로 나눈 것입니다.

- 데이터의 갯수가 증가하여 로드 팩터보다 커지면 해시 재정리 작업을 해야합니다.

  - 저장공간을 확장하고 모든 기존 요소들이 확장한 공간에 재해싱되고 새 버킷에 저장됩니다.

  

#### **HashSet의 주요 메서드 사용법**

### add, addAll - 객체 추가

| 메소드                       | 설 명                                                  |
| ---------------------------- | ------------------------------------------------------ |
| boolean add(Object obj)      | 새로운 객체를 저장한다.                                |
| boolean addAll(Collection c) | 주어진 컬렉션에 저장된 모든 객체들을 추가한다.(합집합) |



``` java 
package HashSet;

import java.util.HashSet;
import java.util.Iterator;
import java.util.Set;

public class HashSetEx {
    public static void main(String[] args) {
        HashSetEx sample = new HashSetEx();
        String[] cars = new String[]{
                "car1", "car2", "car3", "car1",
                "car3", "car4", "car5"
        };
        System.out.println(sample.getCarKinds(cars));
    }

    public int getCarKinds(String[] cars) {
        if(cars == null) return 0;
        if(cars.length == 1) return 1;
        Set<String> carSet = new HashSet<>(cars.length);
        for (String car : cars) {
            carSet.add(car);
        }
//        printCarSet(carSet);
        printCarSet2(carSet);
        return carSet.size();
    }

    public void printCarSet(Set<String> cars) {
        for (String car : cars) {
            System.out.println(car + " ");
        }
    }

    public void printCarSet2(Set<String> cars) {
        Iterator<String> iterator = cars.iterator();
        while(iterator.hasNext()) {
            System.out.println(iterator.next() + " ");
        }
        System.out.println();
    }
}


```



### clear - 모든 데이터 삭제

| 메소드       | 설 명                        |
| ------------ | ---------------------------- |
| void clear() | 저장된 모든 객체를 삭제한다. |

```java
Set set = new HashSet();
 
set.add(1);
set.add(2);
System.out.println(set); // 결과 : [1, 2]
 
set.clear();
System.out.println(set); // 결과 : []
```



### clone - 복제

> [!TIP]
>
> ### 얕은 복사 vs 깊은 복사
>
> - **얕은 복사**: 
>   - 컬렉션에 포함된 객체들의 참조만 복사하고, 실제 객체 자체는 복사하지 않습니다.
>   -  따라서 **복제된 컬렉션과 원본 컬렉션은 동일한 객체에 대한 참조를 공유**하며, 한쪽 컬렉션에서 객체를 변경하면 다른 쪽에도 반영됩니다.
> - **깊은 복사**: 
>   - 컬렉션의 구조뿐만 아니라 컬렉션에 포함된 모든 객체들도 복사합니다. 
>   - 객체가 복제되므로 **복제된 컬렉션에서 변경한 내용이 원본에는 영향을 미치지 않습니다.**



| 메소드         | 설 명                                   |
| -------------- | --------------------------------------- |
| Object clone() | HashSet을 복사해서 반환한다.(얕은 복사) |

```
HashSet set1 = new HashSet();
set1.add(1);
set1.add(2);
System.out.println(set1); // 결과 : [1, 2]
 
HashSet set2 = (HashSet) set1.clone();
System.out.println(set2); // 결과 : [1, 2]
```



## TreeSet

TreeSet은 Set 인터페이스를 구현한 클래스로써 객체를 중복해서 저장할 수 없고 저장 순서가 유지되지 않는다는 Set의 성질을 그대로 가지고 있습니다.

TreeSet은 이진 탐색 트리(BinarySearchTree)의 일종인 **레드-블랙 트리를 사용하여** 해당 요소를 저장합니다.  

![KakaoTalk_20241013_174834869](https://github.com/user-attachments/assets/39c9f96e-9294-41fa-91a1-4bde4b0b2042)

이진 탐색 트리는 추가와 삭제에는 시간이 조금 더 걸리지만 정렬, 검색에 높은 성능을 보이는 자료구조 입니다.

TreeSet은 이진탐색트리의 형태로 데이터를 저장하기에 기본적으로 nature ordering(논리적인 기본순서)을 지원합니다.

``` java
     TreeSet<Integer> ts1 = new TreeSet<>(); // TreeSet 생성

        ts1.add(5);
        ts1.add(3);
        ts1.add(9);
        ts1.add(7);

        System.out.println(ts1); // 출력결과 : [3, 5, 7, 9]
           
    }
```



**생성자의 매개변수로 Comparator 객체를 입력하여 정렬 방법을 임의로 지정해 줄 수도 있습니다.**

``` java
import java.util.TreeSet;
import java.util.Set;

TreeSet<String> set = new TreeSet<>();
TreeSet<Integer> ts1 = new TreeSet<Integer>(); // TreeSet 생성
TreeSet<Integer> ts2 = new TreeSet<>(); // new에서 타입 파라미터 생략 가능
TreeSet<Integer> ts3 = new TreeSet<>(Arrays.asList(1,2,3)); // 초기값 지정;
TreeSet<Integer> ts1 = new TreeSet<>(Comparator.reverseOrder()); // Comparator 입력하여 임의로 내림차순 정렬
```



NavigableSet 인터페이스:

- lower(), floor(), ceiling() 및 higher()와 같은 추가 탐색 메서드를 제공합니다.

``` java
import java.util.TreeSet;
import java.util.Set;

public class NavigationExample {
    public static void main(String[] args) {
        TreeSet<Integer> numbers = new TreeSet<>();
        numbers.add(10);
        numbers.add(20);
        numbers.add(30);
        numbers.add(40);
        numbers.add(50);

        System.out.println("Higher than 25: " + numbers.higher(25)); // 30
        System.out.println("Lower than 30: " + numbers.lower(30));   // 20
        System.out.println("Ceiling of 35: " + numbers.ceiling(35)); // 40
        System.out.println("Floor of 35: " + numbers.floor(35));     // 30
    }
}
// 결과
Higher than 25: 30
Lower than 30: 20
Ceiling of 35: 40
Floor of 35: 30
```

