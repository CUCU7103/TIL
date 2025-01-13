# 고급 동기화 - current.Lock

sychronized는 동기화를 쉽게 할 수있는 매우 편리한 기능이지만 다음과 같은 한계가 있습니다.

- sychronized의 단점
  - 무한 대기 : BLOCKED 상태의 스레드는 락이 풀릴 때 까지 무한 대기합니다.
    - 특정 시간까지만 대기하는 타임아웃 불가
    - 중간에 인터럽트가 불가함.
  - 공정성 :
    - 락이 돌아왔을때 BLOCKED 상태의 여러 스레드 중에 어떤 스레드가 락을 획득할 수 있을지 알 수 없습니다. 
    - 최악의 경우 특정 스레드가 너무 오랜기간 락을 획득하지 못할 수 있다



- 위의 문제를 해결하기 위해서 자바 1.5부터 java.util.concurrent라는 동시성 문제 해결을 위한 패키지가 추가됩니다.



## LockSupport의 기능

- LockSupport 스레드를 WAITING 상태로 변경합니다.
- WAITING 상태는 누가 깨워주기 전까지는 계속 대기한다. 그리고 CPU 실행 스케줄링에 들어가지 않는다



- LockSupport의 대표적인 기능은 다음과 같습니다.
  - park() : 스레드를 WATING 상태로 변경합니다.
    - 스레드를 대기 상태로 둔다.
  - parkNanos(nanos) : 스레드를 나노초 동안만 TIMED_WAITING 상태로 변경합니다.
    - 지정한 나노토가 지나면, TIMED_WAITING 상태에서 빠져나오고 RUNNABLE 상태로 변경됩니다.
  - unPark(thread): 
    - WAITING 상태의 대상 스레드를 RUNNABLE 상태로 변경한다.



``` java
public class LockSupportMainV1 {

	public static void main(String[] args) {
		Thread thread1 = new Thread(new ParkTeset(), "Thread-1");
		thread1.start();

		// 잠시 대기하여 Thread-1이 park 상태에 빠질 시간을 준다.
		sleep(100);
		log("Thread-1 state :" + thread1.getState());

		log("main -> unpark(Thread-1)");
		LockSupport.unpark(thread1); // 1. unpark 사용 , 대상 스레드를 지정해야 합니다.
		// thread1.interrupt(); // 2. interrupt() 사용

	}

	static class ParkTeset implements Runnable {
		@Override
		public void run() {
			log("park 시작");
			LockSupport.park();
			log("park 종료 , state: " + Thread.currentThread().getState());
			log("인터럽트 상태: " + Thread.interrupted());


		}
	}
}

// 결과
22:59:44.625 [ Thread-1] park 시작
22:59:44.726 [     main] Thread-1 state :WAITING
22:59:44.726 [     main] main -> unpark(Thread-1)
22:59:44.727 [ Thread-1] park 종료 , state: RUNNABLE
22:59:44.727 [ Thread-1] 인터럽트 상태: false
```



### 실행상태 그림

![image-20250113225958141](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20250113225958141.png)

- main Thread가 Thread-1을 start() 하면 Thread-1은 Runnable 상태가 된다.
- Thread-1은 Thread.park()를 호출한다. Thread-1은  Runnable -> WAITING 상태가 되면서 대기한다.
- main 스레드가 Thread-1을 unpark()로 깨운다.
- Thread-1은 대기 상태에서 실행가능 상태로 변한다.
  - waiting -> runnable 상태로 변한다.

- 이처럼 LockSupport는 특정 스레드를 WAITING 상태로 RUNNABLE 상태로 변경할 수 있습니다.
- 그런데 대기 상태로 바꾸는 `LockSupport.park()` 는 매개변수가 없는데, 실행 가능 상태로 바꾸는
  `LockSupport.unpark(thread1)` 는 왜 특정 스레드를 지정하는 매개변수가 있을까?
- 왜냐하면 실행 중인 스레드는 `LockSupport.park()` 를 호출해서 스스로 대기 상태에 빠질 수 있지만, 대기 상태의 스레드는 자신의 코드를 실행할 수 없기 때문이다. 따라서 외부 스레드의 도움을 받아야 깨어날 수 있다.



![image-20250113230537457](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20250113230537457.png)



## LockSupport2

- parNanos(nanos): 스레드를 나노초 동안만 TIMED_WAITING 상태로 변경한다.

  - 지정한 나노초가 지나면 TIMED_WAITING 상태에서 빠져 나와서 RUNNABLE 상태로 변경된다.

    

```  java
public class LockSupportMainV2 {

	public static void main(String[] args) {
		Thread thread1 = new Thread(new ParkTest(), "Thread-1");
		thread1.start();

		// 잠시 대기하여 Thread-1이 park 상태에 빠질 시간을 준다.
		sleep(100);
		log("Thread-1 state :" + thread1.getState());

	}

	static class ParkTest implements Runnable {
		@Override
		public void run() {
			log("park 시작");
			LockSupport.parkNanos(2000_000000); // 해당시간이 지난뒤에 깨어난다.
			log("park 종료 , state: " + Thread.currentThread().getState());
			log("인터럽트 상태: " + Thread.interrupted());


		}
	}
}
//
여기서는 스레드를 깨우기 위한 `unpark()` 를 사용하지 않는다.
`parkNanos(시간)` 를 사용하면 지정한 시간 이후에 스레드가 깨어난다.
1초 = 1000밀리초(ms)
1밀리초 = 1,000,000나노초(ns)
2초 = 2,000,000,000나노초(ns)
    
// 실행 결과
23:09:05.539 [ Thread-1] park 시작
23:09:05.630 [     main] Thread-1 state :TIMED_WAITING
23:09:07.547 [ Thread-1] park 종료 , state: RUNNABLE
23:09:07.547 [ Thread-1] 인터럽트 상태: false
```



![image-20250113230934866](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20250113230934866.png)

- Thread-1은 parkNanos(2초)를 사용해서 TIMED_WAITING 상태에 빠진다.
- Thread-1은 2초 이후에 시간 대기 상태를 빠져 나옵니다.

