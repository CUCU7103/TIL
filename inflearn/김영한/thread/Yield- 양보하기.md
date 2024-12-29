# Yield- 양보하기

- 어떤 스레드를 얼마나 실행할지는 운영체제가 스케줄링을 통해서 결정합니다.
- 그런데 특정 스레드가 크게 바쁘지 않은 상황에서 다른 스레드에게 cpu 실행기회를 양보하고 싶을 수 있습니다.
- 이렇게 양보하면 스케줄링 큐에 대기 중인 다른 스레드가 cpu 실행 기회를 더 빨리 얻을 수있습니다.



``` java
public class YieldMain {

	static final int THREAD_COUNT = 1000;

	public static void main(String[] args) {
		for (int i = 0; i < THREAD_COUNT; i++) {
			Thread thread = new Thread(new MyRunnable());
			thread.start();
		}
	}


	static class MyRunnable implements Runnable {
		@Override
		public void run() {
			for (int i = 0; i < 10; i++) {
				System.out.println(Thread.currentThread().getName() + " - " + i);
				// 1. empty
				// sleep(1); // 2. sleep 한 쓰레드가 지속적으로 수행하지 못한다.
				// Thread.yield();
				// 자바의 입장에서는 runnable 상태는 두 종류인데
				// 스케줄링에 들어가서 현재 cpu의 실행을 대기하는 부분
				// 실제 cpu에서 실행되는 부분
			}

		}
	}

}
```

여기서는 3가지 방식을 사용합니다.

1. `Empty` : `sleep(1)` , `yield()` 없이 호출한다. 운영체제의 스레드 스케줄링을 따른다.
2. `sleep(1)` : 특정 스레드를 잠시 쉬게 한다.
3. `yield()` : `yield()` 를 사용해서 다른 스레드에 실행을 양보한다



실행 결과 

- ## empty

  ``` java
  Thread-998 - 2
  Thread-998 - 3
  Thread-998 - 4
  Thread-998 - 5
  Thread-998 - 6
  Thread-998 - 7
  Thread-998 - 8
  Thread-998 - 9
  Thread-999 - 0
  Thread-999 - 1
  Thread-999 - 2
  Thread-999 - 3
  Thread-999 - 4
  Thread-999 - 5
  Thread-999 - 6
  Thread-999 - 7
  Thread-999 - 8
  Thread-999 - 9
  ```

  - 특정 스레드가 쭉 수행된 다음에 다른 스레드가 수행되는 것을 확인할 수 있습니다.
  - 참고로 실행 환경에 따라 결과는 달라질 수 있다. 다른 예시보다 상대적으로 하나의 스레드가 쭉~ 연달아 실행되다가 다른 스레드로 넘어간다
  - 이 부분은 운영체제의 스케줄링 정책과 환경에 따라 다르지만 대략 0.01초(10ms)정도 하나의 스레드가 실행되고, 다른 스레드로 넘어간다



- ## sleep()

  ```  java
  Thread-626 - 9
  Thread-997 - 9
  Thread-993 - 9
  Thread-949 - 7
  Thread-645 - 9
  Thread-787 - 9
  Thread-851 - 9
  Thread-949 - 8
  Thread-949 - 9
  ```

  - sleep(1)을 사용해서 스레드의 상태를 1밀리초 동안 아주 잠깐 `RUNNABLETIMED_WAITING` 으로 변경한다. 
  - 이렇게 되면 스레드는 CPU 자원을 사용하지 않고, 실행 스케줄링에서 잠시 제외된다. 
    - 1 밀리초의 대기 시간 이후 다시 `TIMED_WAITINGRUNNABLE` 상태가 되면서 실행 스케줄링에 포함된다.
  - 결과적으로 `TIMED_WAITING` 상태가 되면서 다른 스레드에 실행을 양보하게 된다. 그리고 스캐줄링 큐에 대기중인 다른 스레드가 CPU의 실행 기회를 빨리 얻을 수 있다.

  - 하지만 이 방식은 `RUNNABLE` `TIMED_WAITING` `RUNNABLE` 로 변경되는 복잡한 과정을 거치고, 또 특정 시간만큼 스레드가 실행되지 않는 단점이 있다.

  - 예를 들어서 양보할 스레드가 없다면, 차라리 나의 스레드를 더 실행하는 것이 나은 선택일 수 있다. 

  - 이 방법은 나머지 스레드가 모두 대기 상태로 쉬고 있어도 내 스레드까지 잠깐 실행되지 않는 것이다.

  - 쉽게 이야기해서 양보할 사람이 없는데 혼자서 양보한 이상한 상황이 될 수 있다.



- ### yield()

  ``` java
  Thread-805 - 9
  Thread-321 - 9
  Thread-880 - 8
  Thread-900 - 8
  Thread-900 - 9
  Thread-570 - 9
  Thread-959 - 9
  Thread-818 - 9
  Thread-880 - 9
  ```



- 자바의 스레드가 `RUNNABLE` 상태일 때, 운영체제의 스케줄링은 다음과 같은 상태들을 가질 수 있다.
- **실행 상태(Running):** 스레드가 CPU에서 실제로 실행 중이다.
- **실행 대기 상태(Ready):** 스레드가 실행될 준비가 되었지만, CPU가 바빠서 스케줄링 큐에서 대기 중이다.
- 운영체제는 실행 상태의 스레드들을 잠깐만 실행하고 실행 대기 상태로 만든다. 그리고 실행 대기 상태의 스레드들을 잠깐만 실행 상태로 변경해서 실행한다. 
- 이 과정을 계속 반복한다. 참고로 자바에서는 두 상태를 구분할 수는 없다



**yield()의 작동**
`Thread.yield()` 메서드는 현재 실행 중인 스레드가 자발적으로 CPU를 양보하여 다른 스레드가 실행될 수
있도록 한다.
`yield()` 메서드를 호출한 스레드는 `RUNNABLE` 상태를 유지하면서 CPU를 양보한다. 즉, 이 스레드는 다시 스
케줄링 큐에 들어가면서 다른 스레드에게 CPU 사용 기회를 넘긴다.
