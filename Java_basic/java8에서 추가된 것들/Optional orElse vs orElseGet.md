# Optional orElse vs orElseGet



## orElse()

- 매개변수로 `T`를 받고 리턴값으로 `T`를 반환합니다.
- 메소드 구현을 보면, 값이 null이 아니면 값을 반환하고  null이면 매개변수를 반환한다.

```java
public T orElse(T other) {
    return this.value != null ? this.value : other;
}
```



## orElseGet()

- 매개변수로 Supplier를 받고 리턴값으로 `T`를 반환합니다.
- **Supplier는 함수형 인터페이스인 람다식을 매개변수로 받습니다.**
- 메소드 구현을 보면, 값이 null이 아니면 값을 반환하고 null이면 Supplier의 `get()`을 호출한다.

```java
public T orElseGet(Supplier<? extends T> supplier) {
    return this.value != null ? this.value : supplier.get();
}
```



### 두 메서드의 가장 큰 차이점은 다음과 같습니다.

- ### orElse () 는 해당 값이 null 이든 아니든 관계없이 항상 불린다.

- ### orElseGet()는 해당 값이 null 일 때만 불린다.

#### 즉 시점의 차이라고 볼 수 있습니다.

``` java
public class TestOptional {
	public static void main(String[] args) {

		Optional<String> optionalValue = Optional.of("실제값");
		String result = optionalValue.orElse(getDefaultValue());
		System.out.println("결과: " + result);

		Optional<String> optionalValue2 = Optional.empty();
		String result2 = optionalValue.orElse(getDefaultValue());
		System.out.println("결과: " + result2);

		System.out.println();
		System.out.println("====orElseGet===");
		Optional<String> optionalValue3 = Optional.of("실제값");
		String result3 = optionalValue3.orElseGet(() -> getDefaultValue());
		System.out.println("결과: " + result3);

		Optional<String> optionalValue4 = Optional.empty();
		String result4 = optionalValue4.orElseGet(() -> getDefaultValue());
		System.out.println("결과: " + result4);

	}
	public static String getDefaultValue() {
		System.out.println("getDefaultValue() 메서드 호출됨");
		return "기본값";
	}

}


// 결과
getDefaultValue() 메서드 호출됨
결과: 실제값
getDefaultValue() 메서드 호출됨
결과: 실제값

====orElseGet===
결과: 실제값
getDefaultValue() 메서드 호출됨
결과: 기본값
    
```



























