# String에서 사용하는 주요한 메서드

- String 클래스는 문자열을 나타냅니다.
- String을 생성하는 방법은
- 문자열 리터럴(literal) 방식과 new 키워드 방식 두 가지가 있다.

해당 내용은 아래에 정리해 두었습니다.
https://github.com/CUCU7103/TIL/blob/main/Java_basic/String%20pool.md

String 클래스는 문자열로 처리할 수있는 다양한 기능을 제공합니다. 
주요한 메서드는 아래와 같습니다. 


문자열의 정보를 조회하는 메서드
length() : 문자열의 길이를 반환한다.
isEmpty() : 문자열이 비어 있는지 확인한다. (길이가 0)
isBlank() : 문자열이 비어 있는지 확인한다. (길이가 0이거나 공백(Whitespace)만 있는 경우)
charAt(int index) : 지정된 인덱스에 있는 문자를 반환한다.

```
package string;

public class StringInfoMain {
    public static void main(String[] args) {
        String str = "Hello, World!";
        System.out.println("문자열의 길이: " + str.length()); 
        System.out.println("문자열이 비어 있는지: " + str.isEmpty());
        System.out.println("문자열이 비어 있거나 공백인지1: " + str.isBlank()); //Java 11
        System.out.println("문자열이 비어 있거나 공백인지2: " + " ".isBlank());
        char c = str.charAt(7);
        System.out.println("7번 인덱스의 문자: " + c);
    }

}
// 결과
문자열의 길이: 13 (공백을 포함)
문자열이 비어 있는지: false
문자열이 비어 있거나 공백인지1: false
문자열이 비어 있거나 공백인지2: true
7번 인덱스의 문자: W

```


문자열을 비교하는 메서드
equals(Object anObject) : 두 문자열이 동일한지 비교한다.
equalsIgnoreCase(String anotherString) : 두 문자열을 대소문자 구분 없이 비교한다.
compareTo(String anotherString) : 두 문자열을 사전 순으로 비교한다.
compareToIgnoreCase(String str) : 두 문자열을 대소문자 구분 없이 사전적으로 비교한다.
startsWith(String prefix) : 문자열이 특정 접두사로 시작하는지 확인한다.
endsWith(String suffix) : 문자열이 특정 접미사로 끝나는지 확인한다.

-compareTo() 비교 대상 문자열의 각 문자들을 첫번째 부터 하나씩 비교하다가 가장 먼저 만나는 다른 문자들의 아스키 코드 값 차이를 반환하고 끝냅니다.

```
public class StringComparisonMain {
    public static void main(String[] args) {
        String str1 = "Hello, Java!"; //대문자 일부 있음
        String str2 = "hello, java!"; //대문자 없음 모두 소문자
        String str3 = "Hello, World!";
        System.out.println("str1 equals str2: " + str1.equals(str2)); // 두 문자열이 같은지 비교합니다.
        System.out.println("str1 equalsIgnoreCase str2: " +
                str1.equalsIgnoreCase(str2)); // 대소문자를 구분하지 않고 비교합니다
        System.out.println("'b' compareTo 'a': " + "b".compareTo("a")); 
        System.out.println("str1 compareTo str3: " + str1.compareTo(str3));
        System.out.println("str1 compareToIgnoreCase str2: " +
                str1.compareToIgnoreCase(str2));
        System.out.println("str1 starts with 'Hello': " +
                str1.startsWith("Hello"));
        System.out.println("str1 ends with 'Java!': " + str1.endsWith("Java!"));
    }

}

// 결과
str1 equals str2: false
str1 equalsIgnoreCase str2: true
'b' compareTo 'a': 1
str1 compareTo str3: -13
str1 compareToIgnoreCase str2: 0
str1 starts with 'Hello': true
str1 ends with 'Java!': true


```

문자열 검색
contains(CharSequence s) : 문자열이 특정 문자열을 포함하고 있는지 확인한다.
indexOf(String ch) / indexOf(String ch, int fromIndex) : 문자열이 처음 등장하는 위치를 반환한다.
lastIndexOf(String ch) : 문자열이 마지막으로 등장하는 위치를 반환한다

```
public class StringSearchMain {

    public static void main(String[] args) {
        String str = "Hello, Java! Welcome to Java world.";
        System.out.println("문자열에 'Java'가 포함되어 있는지: " +
                str.contains("Java"));
        System.out.println("'Java'의 첫 번째 인덱스: " + str.indexOf("Java"));
        System.out.println("인덱스 10부터 'Java'의 인덱스: " + str.indexOf("Java",
                10));
        System.out.println("'Java'의 마지막 인덱스: " + str.lastIndexOf("Java"));
    }
}

// 결과
문자열에 'Java'가 포함되어 있는지: true
'Java'의 첫 번째 인덱스: 7
인덱스 10부터 'Java'의 인덱스: 24
'Java'의 마지막 인덱스: 24
```


문자열 조작 및 변환
substring(int beginIndex) / substring(int beginIndex, int endIndex) : 문자열의 부분 문자열을 반환한다.
concat(String str) : 문자열의 끝에 다른 문자열을 붙인다.
replace(CharSequence target, CharSequence replacement) : 특정 문자열을 새 문자열로 대체한다.
replaceAll(String regex, String replacement) : 문자열에서 정규 표현식과 일치하는 부분을 새 문자열로 대체한다.
replaceFirst(String regex, String replacement) : 문자열에서 정규 표현식과 일치하는 첫 번째 부분을 새 문자열로 대체한다.
toLowerCase() / toUpperCase() : 문자열을 소문자나 대문자로 변환한다.
trim() : 문자열 양쪽 끝의 공백을 제거한다. 단순 Whitespace 만 제거할 수 있다.
strip() : Whitespace 와 유니코드 공백을 포함해서 제거한다. 자바 11

```
public class StringChangeMain1 {
    public static void main(String[] args) {
        String str = "Hello, Java! Welcome to Java";
        System.out.println("인덱스 7부터의 부분 문자열: " + str.substring(7));
        System.out.println("인덱스 7부터 12까지의 부분 문자열: " + str.substring(7,
                12));
        System.out.println("문자열 결합: " + str.concat("!!!"));
        
        System.out.println("'Java'를 'World'로 대체: " + str.replace("Java",
                "World"));
        System.out.println("첫 번째 'Java'를 'World'으로 대체: " +
                str.replaceFirst("Java", "World"));
    }
}
// 결과
인덱스 7부터의 부분 문자열: Java! Welcome to Java
인덱스 7부터 12까지의 부분 문자열: Java!
문자열 결합: Hello, Java! Welcome to Java!!!
'Java'를 'World'로 대체: Hello, World! Welcome to World
첫 번째 'Java'를 'World'으로 대체: Hello, World! Welcome to Java


public class StringChangeMain2 {
    public static void main(String[] args) {
        String strWithSpaces = " Java Programming ";

        System.out.println("소문자로 변환: " + strWithSpaces.toLowerCase());
        System.out.println("대문자로 변환: " + strWithSpaces.toUpperCase());

        System.out.println("공백 제거(trim): '" + strWithSpaces.trim() + "'");
        System.out.println("공백 제거(strip): '" + strWithSpaces.strip() + "'");
        System.out.println("앞 공백 제거(strip): '" + strWithSpaces.stripLeading()
                + "'");
        System.out.println("뒤 공백 제거(strip): '" + strWithSpaces.stripTrailing()
                + "'");
    }
}
// 결과
소문자로 변환:  java programming 
대문자로 변환:  JAVA PROGRAMMING 
공백 제거(trim): 'Java Programming'
공백 제거(strip): 'Java Programming'
앞 공백 제거(strip): 'Java Programming '
뒤 공백 제거(strip): ' Java Programming'

```

문자열 분할 및 조합
split(String regex) : 문자열을 정규 표현식을 기준으로 분할한다.
join(CharSequence delimiter, CharSequence... elements) : 주어진 구분자로 여러 문자열을
결합한다

```
public class StringSplitJoinMain {
    public static void main(String[] args) {
        String str = "Apple,Banana,Orange";
        // split()
        String[] splitStr = str.split(","); // , 기준으로 문자열을 분리한다.
        for(String s : splitStr) {
            System.out.println(s);
        }
        // join()
        String joinedStr = String.join("-", "A", "B", "C");
        System.out.println("연결된 문자열: " + joinedStr);
        // 문자열 배열 연결
        String result = String.join("-", splitStr);
        System.out.println("result = " + result);
    }
}
// 결과
Apple
Banana
Orange
연결된 문자열: A-B-C
result = Apple-Banana-Orange
```

