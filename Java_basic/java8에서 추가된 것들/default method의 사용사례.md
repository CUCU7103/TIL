# Default 메소드의 사용 사례

1. 인터페이스를 확장할때 하위 호환성을 유지합니다.

   - 기존 인터페이스에 새로운 메서드를 추가할 때, 모든 기존 구현 클래스에 영향을 주지 않도록 default 메서드를 사용할 수 있습니다.

     ```` java
     public interface Vehicle {
         void startEngine();
     
         default void stopEngine() {
             System.out.println("Engine stopped.");
         }
     }
     ````

   - 기존 Vehicle 인터페이스를 구현하는 클래스들은 stopEngine 메서드를 별도로 구현하지 않아도 됩니다.

2. 공통 기능의 기본 구현을 제공합니다.

   ``` java
   public interface Drawable {
       void draw();
   
       default void resize() {
           System.out.println("Resizing the drawable object.");
       }
   }
   ```

- `Drawable`을 구현하는 클래스들은 필요에 따라 `resize` 메서드를 오버라이드하지 않고도 사용할 수 있습니다.





========================================================

- 인터페이스를 통한 다중 구현시 동일한 default 메서드가 존재한다면?
  - 컴파일시 오류가 발생한다.
  - 어떤 클래스를 구현해야하는지 명확히 지정해야 하기 때문이다.

- 해결방법

  - 여러 인터페이스에서 동일한 `default` 메서드를 제공할 경우, 클래스가 직접 구현하지 않으면 컴파일 에러가 발생합니다. 
  - 이를 해결하기 위해 클래스에서 명시적으로 어떤 인터페이스의 메서드를 사용할지 지정해야 합니다.

  ``` java
  public interface Flyable {
      default void move() {
          System.out.println("Flying.");
      }
  }
  
  public interface Swimmable {
      default void move() {
          System.out.println("Swimming.");
      }
  }
  
  public class Duck implements Flyable, Swimmable {
      @Override
      public void move() {
          // 특정 인터페이스의 메서드를 명시적으로 호출
          Flyable.super.move();
          Swimmable.super.move();
          System.out.println("Duck moves by flying and swimming.");
      }
  }
  ```

  
