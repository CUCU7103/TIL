# Map이란?

- key와 value를 한 쌍으로 갖는 자료구조입니다.

- HashMap , TreeMap , LinkedHashMap등의 다양한 구현체를 제공합니다.

![image](https://github.com/user-attachments/assets/d98a2433-9a84-43b1-9c4b-049371955f49)


### Map 인터페이스의 주요 메서드

``` 
put(K key, V value) 지정된 키와 값을 맵에 저장한다. (같은 키가 있으면 값을변경)

putAll(Map<? extends K,? extends V> m) 지정된 맵의 모든 매핑을 현재 맵에 복사한다.

putIfAbsent(K key, V value) 지정된 키가 없는 경우에 키와 값을 맵에 저장한다.

get(Object key) 지정된 키에 연결된 값을 반환한다.

getOrDefault(Object key, V defaultValue) 지정된 키에 연결된 값을 반환한다. 키가 없는 경우 defaultValue 로 지정한 값을 대신 반환한다.

remove(Object key) 지정된 키와 그에 연결된 값을 맵에서 제거한다.

clear() 맵에서 모든 키와 값을 제거한다.

containsKey(Object key) 맵이 지정된 키를 포함하고 있는지 여부를 반환한다.

containsValue(Object value) 맵이 하나 이상의 키에 지정된 값을 연결하고 있는지 여부를 반환한다.

keySet() 맵의 키들을 Set 형태로 반환한다.

values() 맵의 값들을 Collection 형태로 반환한다.

entrySet() 맵의 키-값 쌍을 Set<Map.Entry<K,V>> 형태로 반환한다.

size() 맵에 있는 키-값 쌍의 개수를 반환한다.

isEmpty() 맵이 비어 있는지 여부를 반환한다.
```
![image](https://github.com/user-attachments/assets/d5a12dc0-b842-4681-83c6-8553542fbf3a)



- Map 에 키와 값으로 데이터를 저장하면 Map 은 내부에서 키와 값을 하나로 묶는 Entry 객체 를 만들어서 보관합니다.



### HashMap

- value를 저장할때는 key 값으로 해시함수를 실행한 결과를 통해 어느 공간에 저장할지를 결정합니다.
- key 값으로 hashcode()를 실행하고 그 결과값은 해시코드라고 불립니다. 
- 해시 코드를 통해 해시 인덱스를 생성합니다.
- 해시 인덱스가 지정하는 버킷에  key|value의 묶음인 entry를 저장합니다.

> [!NOTE]
>
> - 구조
>   - HashMap 은 해시를 사용해서 요소를 저장한다. 
>   - 키( Key ) 값은 해시 함수를 통해 해시 코드로 변환되고, 이 해시 코드는 데이터를 저장하고 검색하는 데 사용된다.
> - 특징
>   -  삽입, 삭제, 검색 작업은 해시 자료 구조를 사용하므로 일반적으로 상수 시간( O(1) )의 복잡도를 가진다.
>   - 키의 중복은 불가능합니다.
> - 순서
>   - 순서를 보장하지 않는다.



> [!IMPORTANT]
>
> **해싱 (Hashing)** 
>
> - 'HashMap'의 핵심 원리이다.
> - '해싱 함수(Hashing Functoin)'는 **키(key)**를 받아서정수값인 '해시코드(Hashcode)'를 반환하고,
> - 반환된 해시코드는 Hash 배열의 각 요소인 '버킷(Bucket)'의 인덱스가 됩니다.




![image](https://github.com/user-attachments/assets/d5d11244-79a3-49bc-9668-d71b2b107f80)



예시 

``` class Main {
    public static void main(String[] args) {
       Main sample = new Main();
       sample.checkHashMap();
    }

   public void checkHashMap(){
       HashMap<String, String> map = new HashMap();
       map.put("A","a");
       System.out.println(map.get("A"));    
   }

    
}
======================================================================

   public static void main(String[] args) {
        Map<String, Integer> studentMap = new HashMap<>();
        // 학생 성적 데이터 추가
        studentMap.put("studentA", 90);
        studentMap.put("studentB", 80);
        studentMap.put("studentC", 80);
        studentMap.put("studentD", 100);
        System.out.println(studentMap);
        // 특정 학생의 값 조회
        Integer result = studentMap.get("studentD");
        System.out.println("result = " + result);
        System.out.println("keySet 활용");
        Set<String> keySet = studentMap.keySet(); // key값들을 set으로 반환
        for (String key : keySet) {
            Integer value = studentMap.get(key);
            System.out.println("key=" + key + ", value=" + value);
        }
        System.out.println("entrySet 활용");
        Set<Map.Entry<String, Integer>> entries = studentMap.entrySet(); // 맵의 키-값 쌍을 Set<Map.Entry<K,V>> 형태로 반환한다
        for (Map.Entry<String, Integer> entry : entries) {
            String key = entry.getKey();
            Integer value = entry.getValue();
            System.out.println("key=" + key + ", value=" + value);
        }
        System.out.println("values 활용");
        Collection<Integer> values = studentMap.values();
        for (Integer value : values) {
            System.out.println("value = " + value);
        }
    }
```








