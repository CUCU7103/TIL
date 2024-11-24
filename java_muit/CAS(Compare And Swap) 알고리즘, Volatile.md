# CAS(Compare And Swap) 알고리즘, Volatile

> [!NOTE]
>
> # volatile
>
> - 쓰레드는 성능 향상을 위해 매번 Main Memory를 사용하는 것이 아닌 **Cpu Cache를 사용하여 작업을 진행합니다**. 
> - 멀티 쓰레드 환경에서는 C**ache 데이터를 사용하면, 다른 스레드가 데이터를 변경해 Main Memory에는 반영되었지만 알아채지 못하는 상황이 발생하는데 이를 가시성 문제라고 합니다.**
> - 자바에서는 Cache를 사용하지 않고 Main Memory만을 사용하도록 하는 volatile 키워드로 이 문제를 해결합니다.
> - 하지만, volatile로 원자성 문제를 해결할 수 없어 결과적으로 동시성 문제도 해결할 수 없게되는데,
> - 이때 non-blocking 기반 동기화 작업인 CAS 알고리즘을 사용하여 동시성 문제를 해결합니다.

## 가시성 문제

- 하나의 스레드에서 공유자원을 수정한 결과가 다른 스레드에게 보이지 않는 문제를 말합니다.

```JAVA
public class JoinTest2Main {

    public static void main(String[] args) throws InterruptedException {
        SharedResource sharedResource = new SharedResource();

        // 스레드 생성 및 실행
        Thread thread1 = new Thread(sharedResource, "Thread-1");
        thread1.start();

        Thread.sleep(2000); // 메인 스레드가 2초 대기
        System.out.println("Requesting stop...");

        sharedResource.stopRunning(); // stop 값을 true로 변경

        thread1.join(); // 스레드 종료 대기
        System.out.println("Main thread finished.");
    }

}

class SharedResource implements Runnable {
    private volatile boolean stop = false; // volatile 제거/추가로 테스트

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName() + " started.");

        while (!stop) {
            // 실행 중
        }

        System.out.println(Thread.currentThread().getName() + " stopped.");
    }

    public void stopRunning() {
        stop = true; // stop 값을 변경
    }
}
====
volatile 이 없는 경우에는 stop 변수 값이 캐시에서 읽혀지고, true로 변경된 값을 인식하지 못하기 때문에 Thread-1은 무한 루프를 돌게 됩니다.
    
volatile 이 있는 경우에는 stop 변수 값이 volatile로 선언되었기 때문에, Thread-1은 stop 값이 변경된 것을 즉시 감지하고 루프를 종료합니다.
```

이러한 가시성 문제를 해결하는 방법은

- 공유 자원에 ***volatile\*** 키워드를 사용하는 것이고 

- 이는, **항상 Main Memory에 읽고 쓰겠다는 것을 명시하는 것**입니다.

단 한 쓰레드만 쓰기 권한이 있고 그 외 쓰레드는 읽기 권한만 있는 경우에는 데이터의 일관성은 지켜질 수 있으나 2개 이상의 쓰레드가 쓰기 권한이 있다면 동기화가 필요합니다.

> [!NOTE]
>
> 동기화란?
>
> - **멀티스레드 환경에서 여러 스레드가 하나의 공유자원에 동시에 접근하지 못하도록 막는것을 말합니다**



# CAS 알고리즘

Sychcronzied는 blocking 을 사용하여 멀티 스레드 환경에서 공유객체를 동기화 하여 thread-safe 하게 만들어주지만 하나의 스레드가 작업을 마칠때까지 해당 lock에 접근하는 스레드들은 전부 대기해야 하기 때문에 성능저하가 발생하고 락 권한 획득과 해제에 있어서 오버헤드가 발생합니다.

- 이러한 Sychcronized의 단점을 해결하기 위해 사용하는 것이 CAS 알고리즘이다.
- 동시성 처리를 위한 다양한 방법들 (volatile, sychronized, CAS) 중 하나로써 "비교하고 바꿔주는" 알고리즘입니다.
- 컴퓨터 과학(CS) 에서 비동기적 동시성 제어 기법 중 하나로, 여러 스레드가 동시에 데이터를 수정하려고 할 때 데이터의 일관성을 보장하는 방법입니다.

![image](https://github.com/user-attachments/assets/0a84313e-a297-4ffa-9a34-493d0b3cf9ad)


## 원자성과 atomic type

- CAS 알고리즘은 automic 클래스의 핵심 동작원리이다.

- 원자성이란?

  - 어떤 작업이 더 이상 쪼갤 수 없는 단위로 실행됨을 의미합니다.

  - **원자적으로 실행되는 작업은 다른 스레드가 그 작업을 방해할 수 없으며 작업이 완료되면 항상 일관된 상태를 보장**합니다.

    

- ### CAS 알고리즘의 동작 원리는 다음과 같습니다. 

  1. 인자로 기존 값(Compared Value)과 변경할 값(Exchanged Value)을 전달한다.
  2. 기존 값(Compared Value)이 현재 메모리가 가지고 있는 값(Destination)과 같다면 변경할 값(Exchanged Value)을 반영하며 true를 반환한다.
  3. 반대로 기존 값(Compared Value)이 현재 메모리가 가지고 있는 값(Destination)과 다르다면 값을 반영하지 않고 false를 반환한다.

- #### JVM 기준으로  CAS는 다음과 같은 메모리를 사용합니다.

![image](https://github.com/user-attachments/assets/6133a042-f3ab-459c-ae96-b6259bb0c4ec)


- #### **Heap 메모리**

  - 이미지는 `heap` 영역에서 `count`라는 공유 변수가 저장된 것을 보여줍니다.
  - `count`는 JVM의 **Heap 메모리**에 위치하며, 여러 스레드가 동시에 접근 가능한 **공유 데이터**입니다.
  - CAS 알고리즘은 이 **Heap 메모리**의 값을 **원자적으로 비교 및 갱신**합니다.

- **Working Memory (스레드별 캐시)**:

  - 각 스레드(`Thread-1`, `Thread-2`)는 CPU 코어에서 실행되며, **Working Memory(스레드별 캐시)**를 통해 `heap`의 값을 가져옵니다.

  - 이때, CAS는 스레드의 Working Memory에서 가져온 **비교 값(Compared Value)**과 `heap`의 최신 값(Destination)을 비교합니다.

  - Working Memory에 캐싱된 값이 `heap`의 값과 다르면, 변경 작업이 실패하고 `false`가 반환됩니다.

### 정리

- CAS 알고리즘은 **JVM의 Heap 메모리**에 저장된 공유 데이터를 사용합니다.
- 스레드마다 **Working Memory(스레드 캐시)**를 통해 데이터를 읽어오며, 비교 시 주 메모리와 동기화 과정을 거칩니다.
- CAS는 **`heap`의 값(Destination)과 스레드의 캐시 값(Compared Value)을 비교하여, 일치하면 변경하고, 다르면 변경하지 않는 방식으로 동작**합니다.




