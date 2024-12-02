# 단위테스트 vs 통합테스트 vs e2e테스트 (구글 테스트 피라미드)



## 단위 테스트란?

- 응용프로그램에서 테스트 가능한 가장 작은 소프트웨어를 실행하여 예상대로 동작하는지 확인하는 테스트 입니다.

- 애플리케이션 안에 있는 개별적인 코드 단위가 의도한 대로 작동하는지 확인하는 행위
  - 즉 단위 테스트는 개별적인 기능이 우리가 의도한 대로 작동하는지 확인하는 행위입니다.

![image-20241202233336956](C:\typora-image\image-20241202233336956.png)

- 이러한 애플리케이션의 기능들을 하나씩 개별적으로 테스트 하는 것을 의미합니다.

## 단위 테스트를 왜 사용하는가

- 원하는 부분만 테스트를 함으로 결과를 빠르게 볼 수 있습니다.
- 미리 작성한 단위 테스트를 기반으로 프로덕션 코드의 리펙터링을 안전하게 할 수 있습니다.
- 단위 테스트가 실패하는 지점에서 문제점을 빠르게 찾을 수있다.

정리하자면 코드의 일부분을 빠르게 검증할 수 있고, 코드 리펙터링이 안정적이고, 코드의 문제점을 빠르게 찾을 수있습니다.



예시 클래스

``` java
public class Car {
    
    private int position;
    
    public Car(){
        this.position = 0;
    }
    
    
    // 명령한 수 만큼 move() 메서드를 실행시키는 기능
    public void moveAsOrdered(final int orderCount){
        this.position += orderCount;
    }
    
    
    public int getPosition(){
        return position;
    }
    
    
}

==================================================================
 // 단위 테스트
 @Test
 @DisplayName("자동차는 명령을 받은 만큼 이동거리가 증가합니다.")
 void car_moves_as_ordered(){
    int orderCount = 10;
    Car car = new Car();
    car.moveAsOrdered(orderCount);
    int distanceResult = car.getPosition();
    assertThat(distanceResult).isEqualsTo(orderCount);
}
=================================================================
 // given - when - then
 @Test
 @DisplayName("자동차는 명령을 받은 만큼 이동거리가 증가합니다.")
 void car_moves_as_ordered(){
    // given
    int orderCount = 10;
    Car car = new Car();
    // when
    car.moveAsOrdered(orderCount);
    int distanceResult = car.getPosition();
    // then
    assertThat(distanceResult).isEqualsTo(orderCount);
}
 


```

### 결과

![image-20241202234228832](C:\typora-image\image-20241202234228832.png)



테스트 코드의 작성 패턴

1. given - when -then
   - given 
     - 테스트를 위해 준비하는 과정
     - 테스트에 사용되는 값을 정의합니다
   - when
     - 테스트하고자 하는 기능을 실행시킵니다.
   - then
     - 기능을 실행한 후 결과를 검증합니다. 

- 테스트를 더 쉽게 읽을 수 있고 유지보수 하기 좋다
- 필수가 아닌 선택



### @ParameterizedTest

- 여러 값을 한개의 테스트 메서드에서 검증할 수있다.

``` java
@ParameterizedTest
@ValueSource(ints = {-1,0,11,12})
@DisplayName("자동차를 이동시킬 거리가 1 이상 10 이하의 수가 아니면 예외가 발생한다.")
void  throws_exception_when_order_count_invalid(int givenOrderCount){
    // given
    String expectedErrorMessage = "이동시킬 거리는 1 이상 10 이하의 수만 가능하다."
    Car car = new Car();
    
    // when & then 
    assertThatThrownBy(() -> car.moveAsOrdered(givenOrderCount))
        .isInsteadOf(IllegalArgumentException.class)
        .hasMessageContaining(expectErrorMessage);
}
```



### 단위테스트의 원칙 FIRST

- F
  - 단위 테스트는 빨라야 한다.
- Independent
  - 테스트는 독립적으로 동작해야 한다.
  - 이전 테스트에 영향을 받으면 실패원인을 찾기 어렵습니다
- Repeatable 
  - 어떤 상황에서든 예상한 대로 테스트 결과가 나와야 한다.
- Self-Validating
  - 출력 혹은 로그가 아닌 테스트 자체적으로 결과가 나와야 한다.
- Timely
  - 적시에 테스트를 철저하게 작성해야한다.





## 통합테스트란?

- 통합테스트는 단위 테스트 보다 더 큰 동작을 달성하기 위해 여러 모듈들을 모아 이들이 의도대로 협력하는지 확인하는테스트
- 외부 의존성을 수반하는 테스트 입니다.
- 실제 데이터베이스, 파일 시스템, 네트워크 서비스 등과의 상호작용을 포함하여, 더 현실적인 시나리오에서 시스템의 동작을 검증하는 테스트 입니다.



**실제 환경 사용**: 데이터베이스, 웹 서버 등 실제 또는 유사한 환경에서 테스트합니











