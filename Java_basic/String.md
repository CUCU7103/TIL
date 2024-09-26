# String pool

- String의 객체 생성법은 크게 두 가지로 나뉩니다.
- 문자열 리터럴(literal) 방식과 new 키워드 방식입니다.

```
String str1 = "Hello, World!";
String str2 = new String("Hello, World!");
```
**문자열 리터럴 방식이 더 효율적이고 일반적으로 사용됩니다.** 

실제로 코딩하면서 생성자를 사용하여 String을 type을 다룬적은 거의 없었고 문자열 리터럴 방식이 좀 더 직관적이라고 볼 수 있다.
그렇다면 문자열 리터럴 방식의 장점은 직관적인 것 뿐일까?

아니다. 메모리 부분에서도 효율성이 있다.

![image](https://github.com/user-attachments/assets/c3cc8a62-9276-4bfd-a228-705f8fba6930)

리터럴 방식으로 생성된 String은 문자열 풀이라는 특별한 힙 메모리 영역에 위치되어진다.
