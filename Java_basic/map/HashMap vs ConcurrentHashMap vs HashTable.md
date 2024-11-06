# HashMap vs ConcurrentHashMap vs HashTable

#### 모두 자바의 Map 인터페이스를 구현한 클래스들로, 키-값 쌍의 데이터를 저장하고 관리하는데 사용되어집니다.

### 위 3가지 자료구조의 주요한 차이점은 "동기화(synchronized)" 입니다.

> [!NOTE]
>
> ### ***스레드 동기화***는 **<u>멀티스레드 환경에서 여러 스레드가 하나의 공유자원에 동시에 접근하지 못하도록 막는 것</u>을 말합니다.**
>
> #### 즉 Synchronized는 여러 스레드에서 <u>하나의 객체에 있는 인스턴스 변수를 동시에 처리해야 할때 발생하는 문제를 해결하기 위해 사용하는 것</u>입니다.
>
> #### 공유데이터가 사용되어 동기화가 필요한 부분을 <u>임계영역(critical section)</u>이라고 부릅니다.
>
> #### 자바에서는 이 임계영역에 synchronized 키워드를 사용하여 여러 스레드가 동시에 접근하는 것을 금지함(lock이 걸림)으로써 동기화를 할 수 있습니다.
>
> 

- #### **HashMap**: 

  - 동기화를 지원하지 않음으로 Thread safe 하지 않습니다.

  - 따라서 멀티스레드 환경에서는 데이터 무결성이 보장되지 않습니다.

    > [!NOTE]
    >
    > 1. 해시 충돌이 발생하는 경우, 같은 인덱스에 여러 데이터가 중복되어 저장될 수 있습니다. 
    >
    >    이 때는 링크드 리스트를 이용하여 데이터를 추가로 연결하는데, 데이터가 많이 쌓이면 검색 속도가 느려지게 됩니다. 
    >
    >    이런 상황에서 HashMap은 일정 길이 이상의 링크드 리스트를 가지게 되면, 해당 인덱스에 대한 모든 데이터를 새로운 위치로 이동시켜야 하는 데이터의 재배치(rehashing)가 발생합니다.
    >
    > 2. 한 스레드가 HashMap의 값을 수정하는 도중에 다른 스레드가 같은 위치에 있는 값을 수정한다면, 값이 덮어써지는 문제가 발생할 수 있습니다.
    >
    > 3. 여러 스레드에서 동시에 HashMap에 데이터를 추가하면, 배열의 크기가 늘어나야 하는 상황에서도 여러 스레드가 동시에 배열의 크기를 조정하려고 할 수 있습니다.
    >
    >    이런 경우 서로 다른 스레드가 동시에 배열의 크기를 증가시키려고 할 수 있으며, 이로 인해 배열의 크기가 너무 작아져서 데이터의 재배치(rehashing)가 자주 발생하게 됩니다.

    

  - 키는 중복되지 않으며 null을 허용합니다. 값은 중복되어도 상관없으며, null도 허용합니다.

```java
public class HashMap<K,V> extends AbstractMap<K,V>
    implements Map<K,V>, Cloneable, Serializable {

    public V get(Object key) {}
    public V put(K key, V value) {}
}
```



- #### **ConcurrentHashMap** : 

  - 멀티스레드 환경에서의 동시성 문제를 고려하여 설계되었습니다. 
    - Hashtable 클래스의 단점을 보완하면서 Multi-Thread 환경에서 사용할 수 있도록 나온 클래스가 바로 `ConcurrentHashMap` 입니다.
  - 내부적으로 세그먼트나 버킷 단위로 락을 걸어 동시 접근 시에도 높은 성능을 유지합니다



- #### **HashTable**:

  - 모든 메서드에 대해 동기화를 적용하여  Thread safe  을 보장하지만 전체 맵에 대해 락을 걸기 때문에 성능저하가 발생할 수있습니다.
  - null 값을 허용하지 않습니다.

``` java
public class Hashtable<K,V>
    extends Dictionary<K,V>
    implements Map<K,V>, Cloneable, java.io.Serializable {

    public synchronized int size() { }

    @SuppressWarnings("unchecked")
    public synchronized V get(Object key) { }

    public synchronized V put(K key, V value) { }
}
```



### 성능 비교

**`HashMap`:**

- 동기화를 하지 않으므로 단일 스레드 환경에서 가장 빠른 성능을 제공합니다.

**`ConcurrentHashMap`:** 

- 동시성을 고려한 설계로 멀티스레드 환경에서 높은 성능을 유지합니다.

**`Hashtable`:** 

- 모든 메서드에 동기화를 적용하므로 멀티스레드 환경에서 안전하지만, 성능은 상대적으로 떨어집니다
- 지금은 Hashtable 보다 ConcurrentHashMap을 주로 사용합니다.



### 결론 

단일 스레드 환경에서는 HashMap을 사용하고 멀티 스레드 환경에서는 Hashtable보다는 ConcurrentHashMap을 사용하는 것이 좋습니다.





