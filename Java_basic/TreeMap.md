# TreeMap

> [!TIP]
>
> ### TreeMap은 레드-블랙트리로 구성되어져 있습니다.
>
> ![image](https://github.com/user-attachments/assets/44dd556a-02b0-4788-84ed-c04720f77603)


- TreeMap 객체를 저장하면 자동으로 정렬되어집니다.
- 키는 저장과 동시에 오름차순으로 정렬되어집니다
- 숫자의 경우에는 값으로 문자 타입일 경우에는 유니코드로 정렬합니다.
- 키를 정렬하는 작업이 추가되기에 HashMap보다 성능이 떨어집니다.
- 선언 시 크기를 지정할 수 없습니다.



TreeMap은 SortedMap 인터페이스를 구현하고 있어, 데이터를 저장할 때 즉시 정렬하기에 추가나 삭제가 HashMap보다 오래 걸린다. 

하지만 정렬된 상태로 Map을 유지해야 하거나 정렬된 데이터를 조회해야 하는 범위 검색이 필요한 경우에는 TreeMap을 사용하는 것이 효율성면에서 좋다.



실제로 key 값이 정렬되어지는지 확인하기

```java
class Main {
    public static void main(String[] args) {
        Main sample = new Main();
        sample.checkTreeMap();
       
    }

        public void checkTreeMap(){
            TreeMap<String, String> map = new TreeMap();
            map.put("A","a");
            map.put("가","e");
            map.put("1","f");
            map.put("a","g");
            Set<Map.Entry<String,String>> entires = map.entrySet();
            for(Map.Entry<String,String> tempEntry: entires){
                System.out.println(tempEntry.getKey()+ "=" + tempEntry.getValue());
            }
            
        }
}

// 결과
1=f
A=a
a=g
가=e

```



정렬 순서를 변경하고 싶다면?

- **Compartor**구현으로 정렬 순서를 바꿀수 있습니다.

```java
public class Main {
    public static void main(String[] args) {
        // 내림차순으로 정렬하는 Comparator 생성
        Comparator<Integer> comparator = Comparator.reverseOrder();
        
        // TreeMap에 Comparator 전달
        TreeMap<Integer, String> treeMap = new TreeMap<>(comparator);
        
        treeMap.put(1, "One");
        treeMap.put(3, "Three");
        treeMap.put(2, "Two");

        // 출력 (키가 내림차순으로 정렬됨)
        treeMap.forEach((key, value) -> System.out.println(key + " = " + value));
    }
}
```



사용법 자체는 HashMap과 동일합니다.

```java
public class TreeMapMain {

    public static void main(String[] args) {
        TreeMap<Integer,String> map = new TreeMap<>();
        map.put(1, "사과");
        map.put(2, "복숭아");
        map.put(3, "수박");

        // entrySet() 활용
        for(Entry<Integer,String> entry : map.entrySet()){
            System.out.println("key : " + entry.getKey() + " value : " + entry.getValue());
        }

        // KeySet 사용
        for(Integer i : map.keySet()){
            System.out.println("key : " + i + " value : " + map.get(i));
        }

    }

}



===================================================================

TreeMap<Integer,String> map = new TreeMap<Integer,String>(){{//초기값 설정
    put(1, "사과");//값 추가
    put(2, "복숭아");
    put(3, "수박");
}};
		
System.out.println(map); //전체 출력 : {1=사과, 2=복숭아, 3=수박}
System.out.println(map.get(1));//key값 1의 value얻기 : 사과
System.out.println(map.firstEntry());//최소 Entry 출력 : 1=사과
System.out.println(map.firstKey());//최소 Key 출력 : 1
System.out.println(map.lastEntry());//최대 Entry 출력: 3=수박
System.out.println(map.lastKey());//최대 Key 출력 : 3

```































