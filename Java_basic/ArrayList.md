# ArrayList

### List에는 ArrayList, LinkedList가 있습니다.

![image](https://github.com/user-attachments/assets/43c0133b-5135-431f-b969-14f358fee3de)


## Collection 인터페이스 

> [!NOTE]
>
> - Collection 인터페이스는 java.util 패키지의 컬렉션 프레임워크의 핵심 인터페이스 중 하나이다.
> - **자바에서 다양한 컬렉션, 즉 데이터 그룹을 다루기 위한 메서드를 정의한다**
> - **Collection 인터페이스는 List ,  Set , Queue 와 같은 다양한 하위 인터페이스와 함께 사용**되며, 이를 통해 데이터를 리스트, 세트, 큐 등의 형태로 관리할 수 있다. 



## List 인터페이스

> [!NOTE]
>
> - List 인터페이스 List 인터페이스는 java.util 패키지에 있는 컬렉션 프레임워크의 일부다. 
> - **List는 객체들의 순서가 있는 컬렉션을 나타내며, 같은 객체의 중복 저장을 허용한다.**
> - List는 배열과 비슷하지만, 크기가 동적으로 변화하는 컬렉션을 다룰 때 유연하게 사용할 수 있다



## List의 주요한 메서드

| 메소드                                       | 설명                                                        |
| -------------------------------------------- | ----------------------------------------------------------- |
| add(E e)                                     | 리스트의 끝에 지정된 요소를 추가한다                        |
| add(int index, E element)                    | 리스트의 지정된 위치에 요소를 삽입한다                      |
| addAll(Collection<? extends E> c)            | 지정된 컬렉션의 모든 요소를 리스트의 끝에 추가한다          |
| addAll(int index, Collection<? extends E> c) | 지정된 컬렉션의 모든 요소를 리스트의 지정된 위치에 추가한다 |
| get(int index)                               | 리스트에서 지정된 위치의 요소를 반환한다                    |
| set(int index, E element)                    | 지정한 위치의 요소를 변경하고, 이전 요소를 반환한다         |
| remove(int index)                            | 리스트에서 지정된 위치의 요소를 제거하고 그 요소를 반환한다 |
| remove(Object o)                             | 리스트에서 지정된 첫 번째 요소를 제거한다                   |
| clear()                                      | 리스트에서 모든 요소를 제거한다                             |
| indexOf(Object o)                            | 리스트에서 지정된 요소의 첫 번째 인덱스를 반환한다          |
| lastIndexOf(Object o)                        | 리스트에서 지정된 요소의 마지막 인덱스를 반환한다           |
| contains(Object o)                           | 리스트가 지정된 요소를 포함하고 있는지 여부를 반환한다      |
| sort(Comparator<? super E> c)                | 리스트의 요소를 지정된 비교자에 따라 정렬한다               |
| subList(int fromIndex, int toIndex)          | 리스트의 일부분의 뷰를 반환한다                             |
| size()                                       | 리스트의 요소 수를 반환한다                                 |
| isEmpty()                                    | 리스트가 비어있는지 여부를 반환한다                         |
| iterator()                                   | 리스트의 요소에 대한 반복자를 반환한다                      |
| toArray()                                    | 리스트의 모든 요소를 배열로 반환한다                        |
| toArray(T[] a)                               | 리스트의 모든 요소를 지정된 배열로 반환한다                 |



## ArrayList란?

- List 인터페이스를 상속받은 클래스로 크기가 가변적으로 변하는 선형자료구조 입니다.

### ArrayList의 특징 

- 배열을 사용해서 데이터를 관리합니다.
- 기본 CAPACITY(초기 용량)는 10입니다.
- 메모리 고속 복사 연산을 사용한다.
- 배열을 이용하기에 인덱스를 이용해 요소에 빠르게 접근할 수있습니다.
- 크기가 고정되있는 배열과 다르게 가변적으로 공간을 늘리거나 줄일 수있습니다
- **중간 위치에 데이터를 추가하면, 추가할 위치 이후의 모든 요소를 한 칸씩 뒤로 이동시켜야 한다.**



### ArrayList는 세 종류의 생성자를 가집니다.

```  java
// 타입설정 Integer 객체만 적재가능
ArrayList<Integer> members = new ArrayList<>();

// 초기 용량(capacity)지정
ArrayList<Integer> num3 = new ArrayList<>(10);

// 배열을 넣어 생성
ArrayList<Integer> list2 = new ArrayList<>(Arrays.asList(1,2,3));

// 다른 컬렉션으로부터 그대로 요소를 받아와 생성 
// (ArrayList를 인자로 받는 API를 사용하기 위해서 Collection 타입 변환이 필요할 때 많이 사용)
ArrayList<Integer> list3 = new ArrayList<>(list2);

```



### ArrayList의 요소 추가하기

```java
boolean add(Object obj)
=> ArrayList의 마지막에 객체를 추가한다. 추가에 성공하면 true를 반환


void addAll(Collection c)
=> 주어진 컬렉션의 모든 객체를 저장한다.(마지막 index의 뒤로 붙임)

```

```java
public class ArrayListEx1 {

    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>(10); // 용량(capacity)를 10으로 설정

        list.add("A");
        list.add("B");
        list.add("C");
        list.add("D");
        list.add("E");
        list.add("F");

        System.out.println(list.size());         // 크기(size)는 6 (들어있는 요소의 총 개수)
    }
}
```

![image](https://github.com/user-attachments/assets/6e80f3e6-ea00-4dd5-a45c-30f38bd5655b)


##### ArrayList에 요소 추가시 주의 할 점

- 인덱스가 리스트의 capacity를 넘지 않도록 해야합니다.

![image](https://github.com/user-attachments/assets/95e2745e-4d71-4310-9582-98531573ab7c)


- 위 그림에서 물리적인 공간의 크기는(capacity)는 충분하더라도 논리적인 공간은 5이기 때문에 해당 경우 **IndexOutOfBoundsException**이 발생합니다.

  

### ArrayList 요소 삭제

- 요소의 삭제는 마지막에 있는 요소를 삭제하는 경우 O(1), 중간에 있는 요소를 삭제할 경우 O(n)의 시간복잡도를 가진다.

``` java
Object remove(int index)
지정된 위치(index)에 있는 객체를 제거한다.


boolean remove(Object obj)
지정된 객체를 제거한다.(성공하면 true)


boolean removeAll(Collection c)
지정한 컬렉션에 저장된 것과 동일한 객체들을 ArrayList에서 제거한다.


void clear()
ArrayList를 완전히 비운다.


boolean retainAll(Collection c)
ArrayList에 저장된 객체 중에서 주어진 컬렉션과 공통된 것들만 남기고 제거한다.(removeAll 반대 버전)

```

``` java
  public static void deleteSample() {
        ArrayList<String> list = new ArrayList<>(8);

        list.add("1");
        list.add("2");
        list.add("3");
        list.add("4");
        list.add("5");

// 2번째 인덱스 자리의 요소 삭제
        list.remove(2);

        System.out.println(list); // [1, 2, 4, 5]

    }
```



#####  삭제가 발생하는 구조를 표현한 그림

![image](https://github.com/user-attachments/assets/1196f6b4-6a35-4e78-8643-775604f81cf7)


- 만일 리스트 내부의 모든 값들을 제거하려면 clear() 를 사용하여 메서드 내의 모든 정보를 삭제할 수 있습니다.



### **ArrayList 검색**

``` java
boolean isEmpty()
ArrayList가 비어있는지 확인한다.


boolean contains(Object obj)
지정된 객체(obj)가 ArrayList에 포함되어 있는지 확인한다.


int indexOf(Object obj)
지정된 객체(obj)가 저장된 위치를 찾아 반환한다.


int lastIndexOf(Object obj)
지정된 객체(obj)가 저장된 위치를 뒤에서 부터 역방향으로 찾아 반환한다.

```



``` java
  public static void searchList() {
        ArrayList<String> list1 = new ArrayList<>();
        list1.add("A");
        list1.add("B");
        list1.add("C");
        list1.add("A");

    // list에 A가 있는지 검색 : true
        System.out.println(list1.contains("A"));

    // list에 D가 있는지 순차적으로 검색하고 index를 반환 (만일 없으면 -1)
        System.out.println(list1.indexOf("A"));  // 0

    // list에 D가 있는지 연순으로 검색하고 index를 반환 (만일 없으면 -1)
        System.out.println(list1.lastIndexOf("A")); // 3
    }
// 결과 
true
0
3
```





### **ArrayList 인덱스 값 얻기**

```java
Object get(in index)
지정된 위치(index)에 저장된 객체를 반환한다.
=================================
List subList(int fromIndex, int toIndex)
fromIndex부터 toIndex사이에 저장된 객체를 반환한다
```



#### 단일 인덱스값/ 범위 인덱스값 얻어오기

``` java
ArrayList<String> list = new ArrayList<>(18); 

list.add("1");
list.add("2");
list.add("3");
list.add("4");
list.add("5");

list.get(0); // "1"
list.get(3); // "4"
========================================
    
ArrayList<String> list = new ArrayList<>(18); 

list.add("P");
list.add("r");
list.add("o");
list.add("g");
list.add("r");
list.add("a");
list.add("m");

// list[0] ~ list[6] 범위 반환
list.subList(0, 7); // [P, r, o, g, r, a, m]

// list[3] ~ list[6] 범위 반환
list.subList(3, 7); // [g, r, a, m]

// list[3] ~ list[5] 범위 반환
list.subList(3, 6); // [g, r, a]


```

![image-20241012231745011](C:\Users\syste\AppData\Roaming\Typora\typora-user-images\image-20241012231745011.png)



### **ArrayList 요소 변경**

| **메서드**                        | **설 명**                                                    |
| --------------------------------- | ------------------------------------------------------------ |
| Object set(int index, Object obj) | 주어진 객체(obj)를 지정한 위치(index)에 저장한다. 자리에 있던 기존의 데이터는 삭제되고 새로운 데이터가 대체되어 들어간다. |



```java
ArrayList<String> list1 = new ArrayList<>();

list1.add("list1");
list1.add("list1");
list1.add("list1");
 
// index 1번의 데이터를 문자열 "setData"로 변경한다.
list1.set(1, "setData"); 

System.out.println(list1); // [list1, setData, list1]
```



### **ArrayList 용량 확장**

``` java
int size()
ArrayList에 저장된 객체의 개수를 반환한다.


void ensureCapacity(int minCapacity)
ArrayList의 용량이 최소한 minCapacity가 되도록 한다.


void trimToSize()
용량의 크기에 맞게 줄인다 (빈 공간을 없앤다.)

```

``` java
ArrayList<String> list = new ArrayList<>(10); // 용량(capacity)를 10으로 설정

// 용량 10을 넘은 요소 13개 추가
list.add("A");
list.add("B");
list.add("C");
list.add("D");
list.add("E");
list.add("F");
list.add("G");
list.add("H");
list.add("I");
list.add("J");
list.add("K");
list.add("L");
list.add("M");

list.size(); // 크기(size)는 13 : 자동으로 용량이 증가되어 데이터를 적재함

```



### **ArrayList 복사**

```
Object clone()
ArrayList를 복제한다.
```

``` java
ArrayList<Integer> number = new ArrayList<>();
number.add(1);
number.add(3);
number.add(5);

// ArrayList는 내부적으로 Object[] 배열로 저장하기 때문에 형변환이 필요함
ArrayList<Integer> cloneNumber = (ArrayList<Integer>) number.clone();

System.out.println("ArrayList: " + number); // [1, 3, 5]
System.out.println("Cloned ArrayList: " + cloneNumber); // [1, 3, 5]

```



### **ArrayList 배열 변환**

```
Object[] toArray()
ArrayList에 저장된 모든 객체들을 배열로 반환한다.


Object[] toArray(Obejct[] objArr)
ArrayList에 저장된 모든 객체들을 배열 objArr에 담아 반환한다.

```



``` java
ArrayList<String> languages= new ArrayList<>();
languages.add("Java");
languages.add("Python");
languages.add("C");

/* ArrayList<String> 을 String[] 배열로 변환 */

// 방법 1 : 배열로 변환하고 반환
String[] arr1 = languages.toArray();

// 방법 1 : 매개변수로 지정된 배열에 담아 바환
String[] arr2 = new String[languages.size()]; // 먼저 리스트 사이즈에 맞게 배열 생성
languages.toArray(arr2);
```

### **ArrayList 정렬**

- ArrayList를 정렬할때 주의할점은 sort() 메서드는 정렬된 값을 반환하는 것이 아닌, 원본 리스트 자체를 변경 시킨다.

``` java
ArrayList list1 = new ArrayList();
list1.add("3");
list1.add("2");
list1.add("1");
 
// 오름차순 정렬
list1.sort(Comparator.naturalOrder());
System.out.println(list1); // [1, 2, 3]

// 내림차순 정렬
list1.sort(Comparator.reverseOrder());
System.out.println(list1); // [3, 2, 1]
```
