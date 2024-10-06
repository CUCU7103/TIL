# String Builder
- 불변인 String 클래스의 단점은 문자를 더하거나 변경할 때 마다 계속해서 새로운 객체를 생성해야 한다는 점입니다.
- 문자를 자주 더하거나 변경해야 하는 상황이라면 더 많은 String 객체를 만들고, GC해야 하는 상황이 발생할 수있습니다

이러한 String 클래스의 단점을 보완하기 위해 나온것이 StringBuilder 입니다.
```
public class StringBuilderMain {

    public static void main(String[] args) {
        StringBuilder sb = new StringBuilder();
        sb.append("A");
        sb.append("B");
        sb.append("C");
        sb.append("D");
        System.out.println("sb = " + sb);

        sb.insert(4,"java"); // 특정 위치에 문자열 삽입
        System.out.println("insert  = " + sb);

        sb.delete(4, 8); // 특정 범위의 문자열 제거
        System.out.println("delete = " + sb);
        sb.reverse(); // 문자열 뒤집기
        System.out.println("reverse = " + sb);
        //StringBuilder -> String
        String string = sb.toString(); // String으로 변환
        System.out.println("string = " + string);

    }
}

```

 -StringBuilder 는 가변적입니다. 
- 하나의 StringBuilder 객체 안에서 문자열을 추가, 삭제, 수정할수 있으며, 이때마다 새로운 객체를 생성하지 않습니다. 
- 이로 인해 메모리 사용을 줄이고 성능을 향상시킬 수 있습니다. 
- 단 사이드 이펙트를 주의해야 합니다.
- 그리고 문자열을 변경하는 동안에만 StringBuilder를 사용하고 변경작업이 마무리 되면 String으로 변경하는 것이 좋습니다.



