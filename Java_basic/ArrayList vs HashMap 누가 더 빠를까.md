# ArrayList vs HashMap 누가 더 빠를까

## 두 자료구조 중 조회 작업 시 어떠한 자료 구조가 빠를까요?

### 1. ArrayList가 인덱스를 알지 못하는 경우

- **HashMap**은 내부적으로 해시 테이블을 사용하여 데이터를 저장합니다.  
  이를 통해 삽입, 삭제, 수정, 조회 등의 작업에서 일반적으로 **O(1)** 의 시간복잡도를 가집니다.
- 단, **해시 충돌이 자주 발생하는 경우에는 최악의 경우 O(n)까지 복잡도가 증가할 수 있습니다**.
- HashMap은 해시 함수 계산, 해시 충돌 처리, 버킷 관리 등으로 인해 **추가 메모리 오버헤드**가 발생합니다.
- **ArrayList**는 내부적으로 배열을 사용합니다.  
  인덱스를 알지 못하는 경우 원하는 값을 찾으려면 처음부터 끝까지 순차 탐색해야 하므로  
  조회, 삽입, 삭제에는 **O(n)** 의 시간복잡도를 가지게 됩니다.

### 2. ArrayList가 인덱스를 알고 있는 경우

- ArrayList가 인덱스를 알고 있을 경우, HashMap과 동일하게 **O(1)** 의 시간복잡도로 접근할 수 있습니다.
- 하지만 두 자료구조의 **O(1)** 내부 구현 방식에는 차이가 있습니다.

  - **HashMap**
    - 해시 함수를 통해 해시 코드를 생성하고, 이를 기반으로 해시 인덱스를 만들어 해당 버킷에서 값을 찾습니다.
    - 버킷 및 해시 테이블 관리, 해시 충돌 처리 등으로 **추가 메모리 오버헤드**가 발생합니다.
    - 해시 충돌 발생 시에는 최악의 경우 O(n)까지 성능이 저하될 수 있습니다.

  - **ArrayList**
    - 메모리 상의 연속적인 공간에 저장되어 있어, **인덱스 값을 알고 있다면 직접적인 배열 접근으로 항상 O(1)** 의 시간복잡도를 가집니다.
    - 해시 함수 계산, 충돌 처리 등의 추가 연산이 없기 때문에 실제로는 **HashMap보다 더 빠르고 오버헤드가 적습니다**.

### 3. 정리

- **인덱스를 모르는 경우**: HashMap이 평균적으로 훨씬 빠르다 (**O(1)** vs **O(n)**)
- **인덱스를 알고 있는 경우**: ArrayList와 HashMap 모두 **O(1)** 이지만,  
  실제 오버헤드와 일관성 측면에서 **ArrayList가 더 빠르다**고 볼 수 있다.
- **메모리 사용**: HashMap은 해시 테이블과 버킷 등으로 인해 ArrayList보다 더 많은 메모리를 사용한다.


### 4. 예시 코드

``` java
import java.util.ArrayList;
import java.util.HashMap;

public class CollectionSearchDemo {
    public static void main(String[] args) {
        // 데이터 준비
        int dataSize = 1000000;
        ArrayList<Integer> arrayList = new ArrayList<>();
        HashMap<Integer, Integer> hashMap = new HashMap<>();

        // 데이터 추가
        for (int i = 0; i < dataSize; i++) {
            arrayList.add(i);
            hashMap.put(i, i);
        }

        // 찾을 값
        int target = dataSize - 1;

        // 1. ArrayList에서 인덱스를 모를 때 (값으로 탐색: O(n))
        long start = System.nanoTime();
        boolean found = false;
        for (Integer num : arrayList) {
            if (num == target) {
                found = true;
                break;
            }
        }
        long end = System.nanoTime();
        System.out.println("ArrayList (값 탐색, O(n)) 소요 시간: " + (end - start) + "ns, 찾음: " + found);

        // 2. HashMap에서 키로 탐색 (O(1))
        start = System.nanoTime();
        boolean exists = hashMap.containsKey(target);
        end = System.nanoTime();
        System.out.println("HashMap (키 탐색, O(1)) 소요 시간: " + (end - start) + "ns, 찾음: " + exists);

        // 3. ArrayList에서 인덱스로 탐색 (O(1))
        start = System.nanoTime();
        int value = arrayList.get(target);
        end = System.nanoTime();
        System.out.println("ArrayList (인덱스 탐색, O(1)) 소요 시간: " + (end - start) + "ns, 값: " + value);
    }
}
```
