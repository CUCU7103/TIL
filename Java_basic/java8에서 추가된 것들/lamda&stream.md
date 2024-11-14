# JAVA lamda 

- 자바에서 가장 크게 변화된 부분

- **<u>함수형 프로그래밍을 구성하기 위한 함수식이며,  자바의 메소드를 간결한 함수 식으로 표현한 것</u>**입니다.

- 지금까지 자바에서는 메서드를 하나 표현하려면 클래스를 정의해야 했습니다. 

  - 하지만 람다식으로 표현하며 메서드의 이름과 반환값을 생략할 수 있고, 이를 변수에 넣어 자바 코드가 매우 간결해지는 장점이 있습니다.

  ``` java
  int add(int a, int b){
      return a+b;
  }
  =========================
      
   (a, b) -> a + b;   
      
      
  ```

- 특히 컬렉션의 요소를 필터링하거나 매핑하여 원하는 결과를 쉽게 얻을 수 있습니다.
- 인터페이스를 익명 클래스로 구현한 익명 구현 객체를 짧게 표현한 것입니다.



객체 지향 방식 vs 람다 표현 방식

기존의 사용방식

```  java
interface IAdd {
    int add(int x, int y);
}

class Add implements IAdd {
    public int add(int x, int y) {
        return x + y;
    }
}
        
public class Main {
    public static void main(String[] args) {
        // 생 클래스로 메소드 사용하기
        Add a = new Add();
        
        int result1 = a.add(1, 2);
        System.out.println(result1);
    }
}
===================================
    익명 클래스 사용방식
    
    interface IAdd {
    int add(int x, int y);
}

public class Main {
    public static void main(String[] args) {
        // 익명 클래스로 정의해 사용하기 (일회용)
        Iadd a = new IAdd() {
            public int add(int x, int y) {
                return x + y;
            }
        };
        
        int result2 = a.add(1, 2);
        System.out.println(result2);
    }
}

  =======================================
     - 람다식 사용
interface IAdd {
    int add(int x, int y);
}

public class Main {
    public static void main(String[] args) {
        // 람다 표현식으로 함축 하기
        IAdd lambda = (x, y) -> { return x + y; }; // 람다식 끝에 세미콜론을 잊지말자
        
        int result3 = lambda.add(1, 2);
        System.out.println(result3);
    }
}   
    
```

- 인터페이스로 선언한 익명 구현 객체만이 람다식으로 표현이 가능하다. 

  

> [!NOTE]
>
> 함수형 인터페이스란?
>
> - 딱 하나의 추상 메서드가 선언된 인터페이스를 말합니다.
>
>   - default method나 static 메서드는 존재할 수 있습니다.
>
>   ``` java
>   // 함수형 인터페이스
>   interface IAdd {
>       int add(int x, int y);
>   }
>   
>   // 함수형 인터페이스가 될 수 없습니다.
>   interface ICalculate {
>       int add(int x, int y);
>       int min(int x, int y);
>   }
>   ```
>
>   - @FunctionalInterface를 사용하여 명시적으로 함수형 인터페이스 임을 지정할 수있습니다.



기존에 인터페이스의 인스턴스를 생성하려면 구현 클래스를 만들고 추상클래스를 오버라이딩 해야했지만 람다식을 통해 매우 간편하게 변경할 수 있습니다.



그렇다면 람다식을 사용하려면 매번 함수형 인터페이스를 정의해야 할까요?

- 이미 잘 만들어진 표준 함수형 인터페이스가 존재합니다.

![image](https://github.com/user-attachments/assets/d50d374d-8c93-4089-b4ac-62bf19caf223)

또한 람다식이 하나의 메서드만 호출하는 경우에는 메서드 참조로 람다식을 간략히 할 수 있습니다.

![image](https://github.com/user-attachments/assets/dc8ef043-cc26-4994-b8ff-ed890b33450c)




# Stream

- 연속적인 데이터의 흐름을 의미합니다..
- Stream은 **‘일련의 데이터의 흐름’**을 표준화된 방법으로 쉽게 처리할 수 있도록 지원합니다.'
  - 오라클 공식 문서에는 **‘요소들의 Stream에 함수형 연산(람다함수를 통한 연산)을 지원하는 클래스’** 라고 정의하고 있습니다.



``` java
Integer[] values = { 1,3,5};
List<Integer> list = new ArrayList(Arrays.asList.(values));


Integer[] values = { 1, 3, 5};
List<Integer> list = Arrays.stream(values).collect(Collectors.toList());

```

### 스트림의 특징

- 원본 데이터 소스를 변경하지 않는다: 읽기만 한다.
- 일회용이다: 한번 사용하면 닫혀서 재사용이 불가능하다.
- 최종 연산 전까지 중간 연산을 수행하지 않는다.
- 작업을 내부 반복으로 처리한다
  -  forEach()는 매개변수에 대입된 람다식을 데이터 소스의 모든 요소에 적용한다.
- 병렬 처리가 쉽다: 멀티쓰레드 사용
- 기본형 스트림을 제공한다: `Stream<Integer>` 대신 `IntStream`이 제공되어서 오토박싱과 언박싱 등의 불필요한 과정이 생략되고 숫자의 경우 유용한 메소드를 추가로 제공한다. (`.sum()`, `.average()` 등)



### 스트림의 구조

![image](https://github.com/user-attachments/assets/195ea59d-948a-487f-897e-ffb1322f4bf4)


![image](https://github.com/user-attachments/assets/65ea5482-4cf0-4cbb-85c3-f44a5433238b)


1. stream 생성
   - 컬렉션의 목록을 스트림 객체로 변환합니다.
   - stream()
2. 중간연산
   - 생성된 스트림 객체를 사용하여 중간 연산 부분에서 처리합니다. 
   - 이때에는 결과를 return 할 수 없습니다.
3. 최종연산
   - 중간연산에서 작업된 내용을 바탕으로 결과를 리턴해 줍니다.



### 중간 연산의 종류

| Stream 필터           | filter(), distinct() | - filter()는 구성한 스트림 내에서 특정 조건에 맞는 결과값을 필터링 하기 위해 사용됩니다.<br /> - distinct()는 구성한 스트림 내에서 중복을 제거한 결과값을 반환하기 위해 사용됩니다. |
| --------------------- | -------------------- | ------------------------------------------------------------ |
| Stream 변환           | map(), flatMap()     | - map()은 구성한 스트림 내에서 요소들을 원하는 값으로 변환하여서 반환하기 위해 사용됩니다. <br />- flatMap()는 구성한 스트림 내에서 중복된 스트림을 평면화(flattern)하여 요소를 구성하여 반환하기 위해 사용됩니다. |
| Stream 제한           | limit(), skip()      | - limit()는 구성한 스트림 내에서 요소의 개수를 제한하여 반환해주기 위한 목적으로 사용됩니다. <br />- skip()는 구성한 스트림 내에서 시작부터 특정 n개의 요소를 제외하여 반환해주는 목적으로 사용합니다. |
| Stream 정렬           | sorted()             | - sorted()는 구성한 스트림 내에서 배열 혹은 리스트 그리고 Object의 요소에 대해서 오름/내림 차순을 수행하여 반환해주는 함수입니다. |
| Stream 연산 결과 확인 | peek()               | - peek()는 구성한 스트림 내에서 각각의 요소를 조회하는 연산자료 요소 자체를 추출하는것이 아닌 스트림의 요소를 조회하는 함수입니다. |



- Map vs flatMap

1. map

   - **스트림의 각 요소를 특정한 방식으로 변환**하는 역할을 합니다. 

   - **즉 각 요소를 하나하나 매핑(변환)하여 새로운 형태로 만드는 것**

   - 예를 들어, 숫자 리스트를 문자열 리스트로 바꾸거나, 객체의 특정 필드만 추출하는 등의 작업에 사용합니다.

     

   - 결과적으로 원래 스트림과 동일한 수의 요소를 가진 새로운 스트림이 생성됩니다.

``` java
public class StreamTest2 {

	public static void main(String[] args) {

		List<Person> people = Arrays.asList(
			new Person("홍길동", 25),
			new Person("김철수", 30),
			new Person("이영희", 22)
		);

		// 각 사람의 이름만 추출한 리스트 생성
		List<String> names = people.stream()
			.map(Person::getName)
			.collect(Collectors.toList());

		System.out.println(names);
		// 출력: [홍길동, 김철수, 이영희]

	}

}

class Person {
	private String name;
	private int age;

	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}

	public String getName() {
		return name;
	}

	public int getAge() {
		return age;
	}
}

```

- 위 예시를 예로 들어보자면
  - `people.stream()`으로  스트림을 만듭니다.
  - `.map(Person::getName)`을 통해 각 사람의 이름만 추출합니다.
  - `collect(Collectors.toList())`로 결과를 리스트로 수집합니다.

- 스트림을 쓰지 않을때는 for문을 사용해서 요소 하나하나에 접근해야 했지만, map이 반복문의 역할을 대신해주는 것입니다. 



2. flatMap()
   - 각 요소에 함수를 적용하여 생성된 스트림을 하나의 평면 스트림으로 결합합니다. 
     - 리스트의 리스트를 하나의 리스트로 합치기, 문장을 단어로 분리하기
   - 결과적으로 원래 스트림의 요소 수와는 다를 수 있는 새로운 스트림이 생성됩니다.

``` java
public class StreamTest {

	public static void main(String[] args) {

		List<String> sentences = Arrays.asList(
			"안녕하세요",
			"자바 스트림을 사용해요",
			"flatMap을 배우고 있어요"
		);

		List<String> words = sentences.stream()
			.flatMap(sentence -> Arrays.stream(sentence.split(" ")))
			.toList();

		System.out.println(words);

		List<List<String>> friendsHobbies = Arrays.asList(
			Arrays.asList("축구", "독서"),
			Arrays.asList("요리", "영화 감상"),
			Arrays.asList("등산", "사진 찍기")
		);

		List<String> allHobbies = friendsHobbies.stream()
			.flatMap(hobbies -> hobbies.stream())
			.collect(Collectors.toList());

		System.out.println(allHobbies);

	}
    
    

}

// 결과
[안녕하세요, 자바, 스트림을, 사용해요, flatMap을, 배우고, 있어요]
[축구, 독서, 요리, 영화 감상, 등산, 사진 찍기]

```







두 메서드의 차이점을 예시를 통해서 좀 더 명확하게 알아보자

``` java
public class FlatMapWith {
    public static void main(String[] args) {
        List<List<String>> friendsHobbies = Arrays.asList(
            Arrays.asList("축구", "독서"),
            Arrays.asList("요리", "영화 감상"),
            Arrays.asList("등산", "사진 찍기")
        );

        List<String> allHobbies = friendsHobbies.stream()
            .flatMap(hobbies -> hobbies.stream())
            .collect(Collectors.toList());

        System.out.println(allHobbies);
        // 출력: [축구, 독서, 요리, 영화 감상, 등산, 사진 찍기]
    }
}

```

- **스트림 생성**: `friendsHobbies.stream()`으로 외부 리스트의 스트림을 생성합니다.
- **flatMap 적용**: `.flatMap(hobbies -> hobbies.stream())`을 사용하여 각 내부 리스트(`hobbies`)를 스트림으로 변환합니다.
- **평탄화**: `flatMap()`은 이들 **작은 스트림을 하나의 큰 스트림으로 합칩**니다.
- **수집**: `.collect(Collectors.toList())`로 최종 리스트로 만듭니다.



``` java
public class StreamTest3 {

	public static void main(String[] args) {

		List<List<String>> friendsHobbies = Arrays.asList(
			Arrays.asList("축구", "독서"),
			Arrays.asList("요리", "영화 감상"),
			Arrays.asList("등산", "사진 찍기")
		);

		// map() 사용
		List<Stream<String>> mapped = friendsHobbies.stream()
			.map(hobbies -> hobbies.stream())
			.collect(Collectors.toList());

		System.out.println("map() 사용 결과:");
		mapped.forEach(System.out::println);
		// 출력:
		// java.util.stream.ReferencePipeline$Head@...
		// java.util.stream.ReferencePipeline$Head@...
		// java.util.stream.ReferencePipeline$Head@...

		// flatMap() 사용
		List<String> flatMapped = friendsHobbies.stream()
			.flatMap(hobbies -> hobbies.stream())
			.collect(Collectors.toList());

		System.out.println("flatMap() 사용 결과:");
		System.out.println(flatMapped);
		// 출력: [축구, 독서, 요리, 영화 감상, 등산, 사진 찍기]

	}

}
```

#### 즉 두 메서드의 차이점은 아래와 같다고 할 수있다.

- `map()`을 사용하면 **각 내부 리스트가 스트림으로 변환된 스트림의 리스트**가 생성됩니다.

- `flatMap()`을 사용하면 **모든 내부 스트림이 하나의 평탄한 스트림**으로 합쳐져 최종 리스트가 생성됩니다.



### stream vs 반복문

- ### primitive type 에서는 for문이 훨씬 빠릅니다.

  - Effective Java의 저자인 Angelika Langer가 JAX London 2015에서 발표했던 ['The Performance Model of Streams in Java 8"](http://www.angelikalanger.com/Conferences/Videos/Conference-Video-GeeCon-2015-Performance-Model-of-Streams-in-Java-8-Angelika-Langer.html) 에서 그 이유를 찾아보면
  - `for-loop`의 개념 자체는 나온지 이미 오래되었고, 십수년간 JVM에서 내부 최적화가 잘 되어왔다. 한편 `stream`은 java8이 출시된 2015년도부터 나온 개념이기 때문에, 최적화가 for-loop에 비해서 잘 되어있지 않기 때문에 큰 차이가 발생한다고 하였다.

- ### wrapped type에서는 아주 비싼 계산 비용이 필요한 연산에서는 큰 차이가 나지 않습니다.

  - wrapped type은 primitive type과 달리 stack이 아닌 heap 메모리 영역에 저장됩니다.
    - JVM 상에서 Heap Memory에 저장되는 간접 참조는 힙에 객체의 내용이 있다면 중간에 그곳을 가리키는 메모리 영역이 있고 스택에 있는 변수가  메모리 영역을 참조해 실제 힙의 주소를 얻은 후 힙의 실제 내용을 참조하게 되는 방식이다. 
    - 반면, JVM 상에서 Stack Memory에 저장되는 직접 참조의 경우 스택에 있는 변수가 힙 메모리의 실제 주소를 가지고 있어 직접 참조 하게 되는 방식이다.
    - 그러므로 자주 참조되는 변수나 객체가 있다면 직접 참조 방식이 훨씬 뛰어난 성능을 보인다.

- ### 스트림은 for문 보다 간결하게 표현할 수있습니다.

  - 함수형 객체 사용을 지원하므로 표현이 간결해집니다.



- ### for문은  외부반복이고 stream은 내부 반복이다.

  - #### 외부반복 

    -  반복 제어를 프로그래머가 직접 관리하는 방식입니다. 즉, 반복의 각 단계를 명시적으로 제어하고 실행합니다.

    장점

    - 반복의 각 단계를 직접 제어할 수 있어 복잡한 로직이나 조건을 구현하기에 용이합니다.
    - 전통적인 반복문은 많은 개발자에게 익숙하며, 이해하기 쉽습니다.\
    - 반복의 흐름을 자유롭게 변경하거나 중단할 수 있습니다.
      - break, while

    단점

    - 동일한 작업을 수행하기 위해 반복문의 구조를 명시적으로 작성해야 하므로 코드가 길어질 수 있습니다.
    - 병렬 처리를 구현하기 위해 추가적인 코드와 복잡성이 필요합니다.

  

  - #### 내부반복

    - 반복 제어를 컬렉션이나 스트림 API와 같은 추상화된 구조가 담당하는 방식입니다. 
    - 프로그래머는 반복의 "무엇"을 정의하지만 "어떻게" 반복할지는 API가 관리합니다.

  - 장점

    - **간결성**: 데이터 처리 로직을 선언적으로 작성할 수 있어 코드가 간결하고 가독성이 높습니다.
    - **병렬 처리 용이**: 스트림 API를 통해 손쉽게 병렬 처리를 구현할 수 있습니다.
    - **함수형 프로그래밍 지원**: 고차 함수를 사용하여 코드의 재사용성과 모듈화를 촉진합니다.
    - **지연 연산**: 스트림은 필요한 시점에만 연산을 수행하여 성능 최적화에 도움이 됩니다.

  - 단점 

    - **복잡한 로직 처리의 어려움**: 매우 복잡한 조건이나 반복 제어가 필요한 경우 스트림으로 구현하기 어려울 수 있습니다.
    - **디버깅의 어려움**: 스트림의 체인 형태 연산은 디버깅이 어려울 수 있으며, 중간 상태를 확인하기가 힘들 수 있습니다.
    - **추가적인 오버헤드**: 스트림은 내부적으로 객체 생성과 람다 표현식 등을 사용하여 오버헤드가 발생할 수 있습니다.
    - **학습 곡선**: 스트림과 함수형 프로그래밍에 익숙하지 않은 개발자에게는 학습이 필요합니다.

정리해보자면 아래와 같습니다.

**반복문 (외부 반복)**:

- **사용 시기**: 복잡한 로직이나 조건이 필요한 경우, 세밀한 제어가 필요한 경우.
- **장점**: 높은 유연성, 낮은 오버헤드, 직관적인 제어.
- **단점**: 코드가 장황해질 수 있고, 버그 발생 가능성이 높을 수 있음.

**스트림 (내부 반복)**:

- **사용 시기**: 간단하고 일관된 데이터 처리, 병렬 처리의 필요성이 있는 경우, 코드의 간결성과 가독성이 중요한 경우.
- **장점**: 간결한 코드, 병렬 처리의 용이성, 함수형 프로그래밍 지원.
- **단점**: 복잡한 로직 처리의 어려움, 디버깅의 복잡성, 추가적인 오버헤드.

