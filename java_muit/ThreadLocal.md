# ThreadLocal

- 각 스레드마다 독립적인 변수를 유지할 수 있게 해줍니다. 
  - 이는 멀티 스레드 환경에서 스레드 간의 데이터 공유로 인한 문제를 방지하고, 각 스레드가 독립적으로 데이터를 처리할 수 있도록 도와줍니다
- **여러 스레드에서 동시적으로 특정 변수에 접근해서 사용될 때 해당 변수가 다른 스레드에 의해서 변경되어 결과 값이 다르게 나오는 것을 방지하기 위해 사용합니다.**

- **Thread-Local 변수**: 
  - ThreadLocal 객체를 통해 생성된 변수는 각 스레드마다 별도의 복사본을 가지게 됩니다. 
  - 즉, 한 스레드에서의 변경이 다른 스레드에 영향을 미치지 않습니다.

- **Isolation (격리)**: 
  - 각 스레드는 자신의 ThreadLocal 변수에만 접근할 수 있으므로, 데이터의 안전성과 일관성이 보장됩니다.

![image](https://github.com/user-attachments/assets/4dff52c4-c457-43a2-bb94-a589653d698b)


- 동일한 코드를 실행하는 데, 쓰레드1에서 실행할 경우 관련 값이 쓰레드1에 저장되고 쓰레드2에서 실행할 경우 쓰레드2에 저장된다는 점.



### ThreadLocal의 동작 방식

- Thread에서 ThreadLocalMap이란 것을 꺼내서 그 안에서 저장해놓은 변수를 꺼내는 식으로 처리가 되어 있습니다.
- 여기서 Thread의 ThreadLocalMap은 해당 스레드가 갖고 있는 map입니다. 
- ThreadLocal에서 지금 현재 스레드의 table에서 현재 ThreadLocal 클래스를 키 값으로 map에 값을 저장하는 것입니다.

![image](https://github.com/user-attachments/assets/902296e1-6796-454f-83c7-57e2c99c98b3)



**ThreadLocal의 기본 사용법**

1. ThreadLocal 객체를 생성한다.

2. ThreadLocal.initialValue() : ThreadLocal 변수의 초기값을 제공합니다.

   ``` java
   ThreadLocal<String> threadLocal = ThreadLocal.withInitial(() -> "초기값");
   ```

3. ThreadLocal.set() 메서드를 이용해서 현재 쓰레드의 로컬 변수에 값을 저장한다.

4. ThreadLocal.get() 메서드를 이용해서 현재 쓰레드의 로컬 변수 값을 읽어온다.

5. ThreadLocal.remove() 메서드를 이용해서 현재 쓰레드의 로컬 변수 값을 삭제한다.

   ``` java
   ThreadLocal<Integer> threadLocalCounter = ThreadLocal.withInitial(() -> 0);
   
   // 현재 스레드의 카운터 값 설정
   threadLocalCounter.set(10);
   System.out.println("현재 스레드의 카운터: " + threadLocalCounter.get());
   ThreadLocal.remove() // 메서드를 이용해서 현재 쓰레드의 로컬 변수 값을 삭제한다.
   ```



### 주의할 점

- 만약 ThreadLocal에 값을 넣었고, 해당 **Thread**가 **계속** **재사용**된다면 해당 **ThreadLocalMap과 같은 자원을 그대로 가진 채로 동작**하게 된다.

- 그래서 ThreadLocal을 다룰 때는 **사용 이전**, 혹은 **사용 이후**에 반드시 **ThreadLocal**을 **remove()**를 통해 **초기화**하는 처리가 필요하다.



### 주로 활용하는 곳

- 클라이언트 요청에 대해서 각각의 스레드에서 처리할 때나, 스레드 독립적으로 처리해야 하는 데이터와 같이 인증 관련 처리에서도 활용될 수 있다

- #### Spring Security

  - 스프링 시큐리티에서는  SecurityContextHolder에 SecurityContext → Authentication순으로 인증 정보를 보관
  - 여기서 SecurityContextHolder는 SecurityContext를 저장하는 방식을 전략패턴으로 유연하게 대응하는데, 기본 전략이 MODE_THREADLOCAL로 ThreadLocal을 사용하여 SecurityContext를 보관하는 방식입니다. 
