# 인터페이스
-  다른 클래스를 작성할 때 기본이 되는 틀을 제공하는,일종의 추상 클래스를 의미합니다

```
public interface InterfaceAnimal {
  public abstract void sound();
  public abstract void move();
}
```

인터페이스에서 맴버변수는 다음과 같이 구현됩니다.
```

public interface InterfaceAnimal {
  public static final double MY_PI = 3.14;
}

```
- 멤버변수는 public,static,final이 모두 포함되었다고 간주합니다.
- 해당 키워드는 생략이 권장되어집니다.


순수 추상 클래스는 다음과 같은 특징을 가진다고 하였습니다. 여기에 인터페이스는 몇가지 더 추가되었습니다.
- 인스턴스를 생성할 수 없다.
- 상속시 자식은 모든 메서드를 오버라이딩 해야 한다.
- 주로 다형성을 위해 사용된다.
- 인터페이스의 메서드는 모두 public,abstract이다.
- **인터페이스는 다중 구현을 지원한다.**

### 코드 예시
```
public interface InterFaceAnimal {
    void sound();
    void move();
}

public class Cat implements InterFaceAnimal {

    @Override
    public void sound() {
        System.out.println("냐옹");
    }

    @Override
    public void move() {
        System.out.println("고양이 이동");
    }
}
============================================
public class Caw implements InterFaceAnimal {

    @Override
    public void sound() {
        System.out.println("음매");
    }

    @Override
    public void move() {
        System.out.println("소 이동");
    }
}


public class Dog implements InterFaceAnimal {

    @Override
    public void sound() {
        System.out.println("멍멍");
    }

    @Override
    public void move() {

        System.out.println("개 이동");
    }
}
==========================================


public class InterFaceMain {

    public static void main(String[] args) {
        Cat cat = new Cat();
        Dog dog = new Dog();
        Caw caw = new Caw();
        soundAnimal(cat);
        soundAnimal(dog);
        soundAnimal(caw);
    }

    private static void soundAnimal(InterFaceAnimal animal) {
        System.out.println("동물 소리 테스트 시작");
        animal.sound();
        System.out.println("동물 소리 테스트 종료");
    }


}

// 결과
동물 소리 테스트 시작
냐옹
동물 소리 테스트 종료
동물 소리 테스트 시작
멍멍
동물 소리 테스트 종료
동물 소리 테스트 시작
음매
동물 소리 테스트 종료

```
**인터페이스를 사용해야 되는 이유**

- 제약
 - 반드시 메서드를 구현해야 되므로, 개발 시 일관되고 정형화된 개발을 위한 표준화가 가능합니다. 

- 다중구현
  - 자바는 다중 상속을 지원하지 않습니다 이 문제를 구현을 통해서 해결할 수 있습니다.
  



  
