## Queue

- 선입선출(FIFO)의 성격을 가진 자료구조 입니다.
  - 삽입 순으로 나열되어 가장 먼저 삽입한 원소가 가장 먼저 삭제된다.

![image](https://github.com/user-attachments/assets/064d5192-871e-4c49-b815-506974f6c497)


## LinkedList

- 자바의 LinkedList는 ArrayList와 같이 인덱스로 접근하여 조회, 삽입이 가능하지만 내부 구조가 완전히 다르게 구성되어 있다는 점이 특징입니다
- ArrayList는 내부적으로 배열을 이용하여 조작이 가능하게 만든 컬랙션
- Linked 리스트는 노드(객체) 끼리의 주소 포인터를 서로 가르키며 링크(참조) 함으로써 이어지는 구조입니다.
- 이중 연결 목록입니다. 즉, 각 노드에는 다음 노드와 이전 노드에 대한 참조가 모두 있습니다. 이를 통해 목록의 양쪽 끝에서 효율적인 삽입과 삭제가 가능합니다.

## **Java의 LinkedList를 사용해야 하는 경우**

자주 삽입/삭제: 

- 응용 프로그램이 목록의 시작이나 중간에 자주 삽입하거나 삭제해야 하는 경우에 적합합니다.
- 큐/디큐 구현: 큐와 유사한 기능이 필요할 때 유용합니다.

![image](https://github.com/user-attachments/assets/7387c4cd-b5e1-46d4-9ecd-3d6c21db3dd7)


### LinkedList 객체 생성

```
LinkedList()
-> LinkedList객체를 생성

LinkedList(Collection c)
-> 주어진 컬렉션을 포함하는 LinkedList객체를 생성

```

- ArrayList처럼 초기 값을 미리 지정하는 기능은 제공되지 않는다. 

### 주요 메서드

```java
요소 추가:
add(E e): 지정된 요소를 끝에 추가합니다.
add(int index, E element): 지정된 위치에 요소를 삽입합니다.
addFirst(E e): 요소를 처음에 삽입합니다.
addLast(E e): 요소를 끝에 추가합니다.

요소 제거:
remove(): 첫 번째 요소를 제거하고 반환합니다.
remove(int index): 지정된 위치의 요소를 제거합니다.
removeFirst(): 첫 번째 요소를 제거합니다.
removeLast(): 마지막 요소를 제거합니다.

요소 검색 중:
get(int index): 지정된 위치에 있는 요소를 반환합니다.
getFirst(): 첫 번째 요소를 반환합니다.
getLast(): 마지막 요소를 반환합니다.

기타 방법:
size(): 요소 수를 반환합니다.
clear(): 모든 요소를 제거합니다.
contains(Object o): 목록에 지정된 요소가 포함되어 있으면 true를 반환합니다.
iterator(): 요소에 대한 반복자를 반환합니다.
```



### 사용예시

``` java
import java.util.*;
import java.lang.*;
import java.io.*;

// The main method must be in a class named "Main".
class Main {
    public static void main(String[] args) {

        Main sample = new Main();
        sample.checkedLinkedList1();
           
    }

    public void checkedLinkedList1(){
        LinkedList<String> link = new LinkedList<String>();
        link.add("A");
        link.add("B");
        System.out.println(link);
        link.offerFirst("C");
        System.out.println(link);
        link.addLast("D");
        System.out.println(link);
        link.offer("E");
        System.out.println(link);
        link.offerLast("F");
        System.out.println(link);
        link.push("G");
        System.out.println(link);
        link.add(0,"H");
        System.out.println(link);
        System.out.println("Ex =" + link.set(0,"I"));
        System.out.println(link);
    }    
}

// 결과
[A, B]
[C, A, B]
[C, A, B, D]
[C, A, B, D, E]
[C, A, B, D, E, F]
[G, C, A, B, D, E, F]
[H, G, C, A, B, D, E, F]
Ex = H
[I, G, C, A, B, D, E, F]


```



## LinkedList 스택 & 큐 지원

 `Deque` 를 구현하므로써 stack과 queue 로서도 이용이 가능합니다. 그래서 아래와 같이 스택과 큐에 관한 전용 메서드를 별도로 제공합니다.
 
![image](https://github.com/user-attachments/assets/bd30ea51-fcb1-4202-b9b9-3ebe5bb98f86)










