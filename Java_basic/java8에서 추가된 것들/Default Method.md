# Default Method 

- 자바에서는 인터페이스 안에 정의된 메서드들은 인터페이스를 구현하는 클래스들이 반드시 메서드 구현을 해야했습니다. 
- 만일 인터페이스에 새로운 메서드(newMethod)를 추가한다고 하면 그 인터페이스를 구현하는 모든 메서드는 새로 추가되어진 메서드를 구현해야 합니다.


![image](https://github.com/user-attachments/assets/63f35330-25a9-4c1b-ac34-3e578b8ae4c4)




- 즉 새로운 메서드가 필요없는 구현 클래스들이 새로운 메서드를 구현해야 한다는 문제점이 발생합니다.
- 이 문제는 OCP 원칙에 위배되어집니다.

> [!NOTE]
>
> 개방 폐쇄 원칙이란? (Open/closed principle)
>
> - 소프트웨어 요소는 확장에는 열려 있으나 변경에는 닫혀 있어야 한다.
> - 소프트웨어 구성 요소(컴포넌트, 클래스, 모듈, 함수)는 **확장에** 대해서는 **개방**(**OPEN**)되어야 하지만 **변경에** 대해서는 **폐쇄**(**CLOSE**)되어야 한다는 의미

- 위 문제의 경우는 확장에는 개방되어져 있지만 변경에 대한 폐쇄를 위반한 클래스라고 볼 수있습니다.

- 새로운 메서드가 interface에 추가된다는 이유로 추가된 메서드를 사용하지 않는 모든 구현 클래스들이  강제적으로 메서드를 구현해야 되기 때문입니다.

  

## default method 란?

- 인터페이스에 있는 구현 메서드를 말하는 것입니다.
- **인터페이스에 기본 구현을 제공**할 수 있으며, **구현 클래스에서 해당 메서드를 오버라이드하지 않아도 됩니다.**

기존의 추상 메서드(abstract method)와 다른 점은 

- 메서드 앞에 예약어  default가 붙어야 합니다.
- 구현부 `{}`가 있어야 합니다.

``` java
public interface MyInterface {
    // 추상 메서드
    void abstractMethod();

    // 기본 메서드
    default void defaultMethod() {
        // 기본 구현 코드
        System.out.println("This is a default method.");
    }
}

public class TestDefaultMethod implements TestInterface{

	@Override
	public void abstractMethod() {
		System.out.println("abstractMethod");
	}

	public static void main(String[] args) {
		TestDefaultMethod testDefaultMethod = new TestDefaultMethod();
		testDefaultMethod.abstractMethod();
		testDefaultMethod.defaultMethod();
	}
}

// 결과
abstractMethod
This is a default method.

```



## **활용** 

1. **인터페이스의 기능 확장(유연성, 확장성)**
   - 기존 인터페이스를 수정하지 않고도 새로운 기능을 추가할 필요가 있는 경우에 기본 메서드를 사용

2. **라이브러리와 호환성 유지**

   - Java 8 이후의 라이브러리들은 기본 메서드를 활용하여 인터페이스를 설계하고 있습니다.

   -  라이브러리의 개발자는 새로운 기능을 추가하거나 수정하면서도, 기존 사용자 코드와의 호환성을 유지할 수 있다.

3. **인터페이스의 다중 상속**
   - 여러 인터페이스에서 동일한 기본 메서드를 가질 수 있으며, 이를 구현하는 클래스에서 필요에 따라 해당 메서드를 오버라이드하여 자체 구현을 제공할 수 있습니다. (다중 상속의 이점을 활용하면서도 충돌을 해결)

4. **인터페이스의 선택적 구현**
   - 모든 메서드를 구현해야 하는 것이 아니라 필요한 메서드만 구현할 수 있다. 사용자에게 구현 부담을 줄이고, 필요한 기능만 구현하는 유연성을 제공











