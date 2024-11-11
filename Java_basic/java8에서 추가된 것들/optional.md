# Optional

### Optional은 프로그래밍에서 값이 존재할 수도 있고 존재하지 않을 수도 있는 변수를 표현하는 데 사용되는 개념입니다.

#### Optional 객체를 사용하면 예상치 못한 NullPointerException 예외를 제공되는 메소드로 간단히 회피할 수 있습니다.

- 즉, 복잡한 조건문 없이도 널(null) 값으로 인해 발생하는 예외를 처리할 수 있게 됩니다



API 공식 문서에 따르면 아래와 같이 설명하고 있습니다.

[https://docs.oracle.com/javase/9/docs/api/java/util/Optional.html]

> [!NOTE]
>
> - 메소드가 반환할 결과 값이 '없음'을 명백하게 표현할 필요가 있고, null 을 반환하면 에러가 발생할 가능성이 높은 상황에서 메소드의 반환 타입으로 Optional 을 사용하자는 것이 Optional 을 만든 주된 목적이다. 
>
> - Optional 타입의 변수의 값은 절대 null 이어서는 안 되며, 항상 Optional 인스턴스를 가리켜야 한다.



### Optional의 사용목적

- **메서드의 리턴 타입으로** **‘결과 없음’을 명확히 표현하는 용도로 제한적으로 사용하기 위한 용도로 제공**됩니다.

- **Java8** 이전에는 Optional이라는 클래스가 없었으므로, **메서드의 리턴 타입으로** **‘결과 없음’** 을 표현하기 위한 용도로 null이 사용되었습니다. 

- 그러나 객체 값이 null인 경우에, null 체크 없이 해당 객체의 메서드를 호출하거나 하는 등, **부주의하게 사용하는 경우에 ‘NPE(NullPointerException)’이 발생하면서 프로그램이 죽어버리는 상황이 발생했습니다.**

- 이런 상황을 방지하고 안전한 코딩을 위해서 Optional이 만들어지게 되었습니다. 

  **null 값을 반환해야 하는 경우에 Optional로 감싼 객체로 empty 값을 표현하여 결괏값을 넘겨주면, null로 인한 오류 문제를 없앨 수 있습니다.**



Optional의 생성방법

- **Optional에서 제공하는 static 메서드를 사용하여 Optional을 만들어 낼 수 있습니다.** .

``` java
Optional<String> empty = Optional.empty(); // 값이 비어있는 Optional을 생성방법

Optional<String> opt = Optional.of(“Hello”); // “Hello”란 값이 담겨있는 Optional을 생성

String temp = null;

Optional<String> opt = Optional.ofNullable(temp) // 인자(temp)가 null일 수도 있는 상황에서 사용
```

- of() 메서드를 사용하는 경우, null을 인자로 사용하게 되면, NullPointerException 예외가 발생하므로 null 일 가능성이 있는 값을 인자로 받는 경우에는 ofNullable() 메서드를 사용해야 합니다



### **Optional이 제공하는 메서드와 사용방법**

- #### **isPresent()**

  - Optional의 값이 존재하면 true, 해당 값이 없으면 false를 리턴합니다.
     

- #### **isEmpty()**

  - isPresent() 메서드와는 반대로 체크하는 메서드로 Optional의 값이 비어있으면 true, 해당 값이 존재하면 false를 리턴합니다. 단, isEmpty() 메서드는[ java](https://www.elancer.co.kr/blog/view?seq=173)11 버전부터 사용이 가능합니다.
     

- #### **get()**

  - Optional이 감싸고 있는 실제 값을 얻기 위해서 사용하며, 만일 Optional의 값이 비어있는 경우, NoSuchElementException이 발생하게 됩니다. 

  - 그러므로, Optional의 값이 항상 있다고 판단되지 않는다면 isPresent() 또는 isEmpty()로 값의 존재 여부를 체크한 뒤, get() 메서드를 사용하는 형태로 사용해야 합니다.

``` java
Optional<String> optionalString = Optional.of("Hello");

if (optionalString.isPresent()) {    // 값이 존재하는지 확인

    System.out.println("Optional contains a value: " + optionalString.get());

} else {

    System.out.println("Optional is empty");

}
```



- #### **ifPresent(), ifPresentOrElse()**

  - **ifPresent() 메서드를 이용해서 값이 있는 경우, 처리하는 함수를 직접 람다 함수로 사용**할 수 있으며, ifPresentOrElse()를 사용해서 **값이 있는 경우와 없는 경우를 동시에 처리할 수 있는 메서드도 제공**합니다. 

``` java
Optional<String> optionalString = Optional.of("Hello");

optionalString.ifPresent( val -> System.out.println("Optional contains a value: " + val),

() -> System.out.println("Optional is empty") );
```

- 위와 같이 작성하면 if 문을 사용하는 것보다 코드를 간략하게 작성 가능한 장점이 있습니다.





- #### **orElse(), orElseGet(), orElseThrow()**

``` java
Optional<String> optionalString = Optional.of("Hello");

String value = optionalString.orElse("Default Value"); // 대체값 지정
 
// 대체값을 람다함수로 지정
String result = optionalString.orElseGet(() -> "Value from Supplier");

// 값이 없을 때 예외 던지기
String str = emptyOptional.orElseThrow(() -> new RuntimeException("Optional is empty"));
```

- Optional 클래스는 값이 없는 경우(empty), 대체 값이나 예외를 발생시킬 수 있는 orElse(), orElseGet(), orElseThrow()  메서드를 제공하고 있습니다. 



Java 9 부터 추가된 or() 메서드를 사용하면, orElseGet()이나 orElse()와 달리 Optional 객체를 리턴할 수 있습니다.



 **or()**

``` java
Optional<String> firstOptional = Optional.empty();

Optional<String> secondOptional = Optional.of("Hello, world!");

// 두 Optional 객체 중 하나라도 값이 존재하는 경우 값을 반환하고, 모두 값이 없는 경우 "Default Value"를 반환

String result = firstOptional.or(() -> secondOptional).orElse("Default Value");

System.out.println("Result: " + result);
```

- 위의 result 값은 firstOptional의 값이 없으므로, or() 메서드의 람다 함수가 실행되어 secondOptional이 리턴되고, secondOptional의 값은 존재하기 때문에, “Hello world!”가 됩니다.

 

- #### **filter()**

  - Optional의 값에 filter를 적용하여 조건에 맞는 값인 경우에 사용할 경우, filter()를 사용할 수 있습니다.

```  java
Optional<String> optionalString = Optional.of("Hello World");

// filter를 사용하여 문자열의 길이가 5 이상인 경우에만 처리

optionalString.filter(s -> s.length() >= 5)

.ifPresent(s -> System.out.println("Filtered Value: " + s));
```



 

- #### **map()**

  - Optional의 값을 변환해서 사용해야 하는 경우, map()을 이용해 기존 값을 변환하여 값을 가져올 수 있습니다.

``` java
Optional<String> optionalString = Optional.of("Hello World");

// filter를 사용하여 문자열의 길이가 5 이상인 경우에만 처리

optionalString.filter(s -> s.length() >= 5)

.ifPresent(s -> System.out.println("Filtered Value: " + s));
```



 

- #### **flatMap()**

  - flatMap()은 변환한다는 면에서는 map()과 동일하지만, map()이 인자로 받는 람다 함수는 리턴 값이 객체이며, 이 객체를 Optional로 감싸져서 return 하게 되지만, 
  - **flatMap()이 인자로 받는 람다 함수는 리턴 값이 객체가 아니라** **객체를 감싸는 Optional 객체를 리턴한다는 점**에서 차이가 있습니다.

``` java
// flatMap을 사용하여 문자열을 Optional로 감싼 후 해제

Optional<String> optionalString = Optional.of("Java Optional");

Optional<String> flatMappedOptional = optionalString.flatMap(s -> Optional.of(s + " Tutorial"));

flatMappedOptional.ifPresent(s -> System.out.println("FlatMapped Value: " + s));
```

- 즉, Optional 값 변환 시, 람다 함수의 리턴 값이 Optional인 경우에는 flatMap을 사용하고, 그렇지 않은 경우에 map()을 사용하면 됩니다.



### **1) Optional 변수에 null을 할당하지 않는다.**

**Optional은 null을 사용하지 않기 위해서 만들어진 클래스**인데, **Optional을 null로 초기화하면 Optional의 사용 의도와 맞지 않게 됩니다.** 빈 값으로 Optional을 만들려면 Optional.empty()로 객체를 생성해야 합니다.

 

### **2) Optional을 필드 타입으로 사용하지 않는다**

Optional은 메서드의 리턴 타입으로 사용하기 위한 용도로 설계되었기 때문에, **Optional을 클래스의 필드로 선언하는 것은 잘못된 사용 방식**이며, **필드 타입은 객체를 그대로 사용하는 것이 올바른 사용 방식**입니다.

 

### **3) 메서드의 인자로 Optional을 사용하지 않는다.**

Optional은 메서드의 리턴 타입으로 사용하기 위한 용도이므로, **Optional을 메서드의 인자로 사용하는 것도 잘못된 사용 방식**으로 **메서드의 인자도 역시 객체를 그대로 사용하는 것이 올바른 사용 방식**입니다.

 

### **4) 빈 컬렉션이나 배열을 나타낼 때는 Optional을 사용하지 않는다.**

**빈 컬렉션을 리턴하고 자 하는 경우는 Optional을 사용할 것이 아니라,** **빈 컬렉션을 리턴하는 것이 올바른 사용 방식**입니다. 

ArrayList나 HashMap() 등은 최초 객체를 생성하면 비어있는 컬렉션이 생성이 되므로 이를 사용해서 리턴하면 됩니다.

컬렉션이 ‘비어있음’을 나타내기 위해서 굳이 Optional을 사용하는 것은 적합한 사용 방식이 아닙니다. 마찬가지로 배열의 경우도 ‘비어있음’을 나타내려면 빈 배열을 생성하여 사용하면 됩니다.

















