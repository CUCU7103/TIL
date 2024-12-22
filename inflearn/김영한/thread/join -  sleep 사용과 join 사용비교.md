# Join - sleep 사용과 join 사용비교

``` java
package thread.control.join;

import static util.MyLogger.*;
import static util.ThreadUtils.*;

public class JoinMainV2 {

	public static void main(String[] args) {
		log("Start");
		SumTask task1 = new SumTask(1, 50);
		SumTask task2 = new SumTask(51, 100);

		Thread thread1 = new Thread(task1, "thread-1");
		Thread thread2 = new Thread(task2, "thread-2");

		thread1.start();
		thread2.start();

		// 정확한 타이밍을 맞추어 기다리기가 어렵습니다.
		log("main thread sleep() ");
		sleep(3000);
		log("main thread 깨어남");

		log("task1.result = " + task1.result);
		log("task2.result = " + task2.result);

		int sumAll = task1.result + task2.result; // task1,2 스레드가 작업을 완료하기도 전에 결과를 구함
		log("sumAll = " + sumAll);

		log("End");

	}


	static class SumTask implements Runnable{

		int startValue;
		int endValue;
		int result = 0;

		public SumTask(int startValue, int endValue) {
			this.startValue = startValue;
			this.endValue = endValue;
		}

		@Override
		public void run() {
			log("작업시작");
			sleep(2000);
			int sum = 0;
			for(int i = startValue; i <= endValue;  i++){
				sum += i;
			}
			result = sum;
			log("작업 완료 result = " + result );
		}
	}
/**
*01:08:38.054 [     main] Start
01:08:38.056 [     main] main thread sleep() 
01:08:38.056 [ thread-1] 작업시작
01:08:38.056 [ thread-2] 작업시작
01:08:40.059 [ thread-2] 작업 완료 result = 3775
01:08:40.059 [ thread-1] 작업 완료 result = 1275
01:08:41.063 [     main] main thread 깨어남
01:08:41.063 [     main] task1.result = 1275
01:08:41.063 [     main] task2.result = 3775
01:08:41.063 [     main] sumAll = 5050
01:08:41.064 [     main] End
/

}

```

- 특정 스레드를 기다리게 하는 가장 간단한 방법은 sleep을 사용하는 것입니다.

![image-20241223010908043](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20241223010908043.png)

- thread-1 , thread-2는 계산에 2초 정도의 시간이 걸리고 우리는 이 부분을 알고있기에 main 스레드가 3초후에 계산 결과를 조회하도록 했습니다 
- 하지만 다른 경우에 시간이 얼마나 걸리는지를 매번 알고 있는 경우는 드물것입니다.
- 또 각 스레드의 수행시간이 변경된다면 정확한 타이밍을 맞추기 어렵습니다.

### 이럴때 join을 사용할 수있습니다.

``` java
package thread.control.join;

import static util.MyLogger.*;
import static util.ThreadUtils.*;

public class JoinMainV3 {

	public static void main(String[] args) throws InterruptedException {
		log("Start");
		SumTask task1 = new SumTask(1, 50);
		SumTask task2 = new SumTask(51, 100);

		Thread thread1 = new Thread(task1, "thread-1");
		Thread thread2 = new Thread(task2, "thread-2");

		thread1.start();
		thread2.start();

		// 스레드가 종료될 때 까지 대기
		log("join() - main 스레드가 thread1, thread2 종료까지 대기");
		thread1.join();
		thread2.join();
		log("main 스레드 대기 완료");

		log("task1.result = " + task1.result);
		log("task2.result = " + task2.result);

		int sumAll = task1.result + task2.result; // task1,2 스레드가 작업을 완료하기도 전에 결과를 구함
		log("sumAll = " + sumAll);

		log("End");

	}


	static class SumTask implements Runnable{

		int startValue;
		int endValue;
		int result = 0;

		public SumTask(int startValue, int endValue) {
			this.startValue = startValue;
			this.endValue = endValue;
		}

		@Override
		public void run() {
			log("작업시작");
			sleep(2000);
			int sum = 0;
			for(int i = startValue; i <= endValue;  i++){
				sum += i;
			}
			result = sum;
			log("작업 완료 result = " + result );
		}
	}
	
}

```



![image-20241223011114353](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20241223011114353.png)

- join()을 사용하게 되면 main 스레드는 thread-1, thread-2가 종료될때 까지 기다립니다.
  - 이때 main 스레드의 상태는 waiting 상태가 됩니다.

```java
thread1.join();
thread2.join();
```

- 예를 들어서 `thread-1` 이 아직 종료되지 않았다면 `main` 스레드는 `thread1.join()` 코드 안에서 더는 진행하지 않고 멈추어 기다립니다.
-  이후에 `thread-1` 이 종료되면 `main` 스레드는 `RUNNABLE` 상태가 되고 다음 코드로 이동합니다.
- 이때 `thread-2` 이 아직 종료되지 않았다면 `main` 스레드는 `thread2.join()` 코드 안에서 진행하지 않고 멈추어 기다립니다.
- 이후에 `thread-2` 이 종료되면 `main` 스레드는 `RUNNABLE` 상태가 되고 다음 코드로 이동합니다.
- 이 경우 `thread-1` 이 종료되는 시점에 `thread-2` 도 거의 같이 종료되기 때문에 `thread2.join()` 은 대기하지 않고 바로 빠져나옵니다.

#### Waiting 상태란?

> [!TIP]
>
> - 스레드가 다른 스레드의 특정 작업이 완료되기를 무기한 기다리는 상태이다.
>   `join()` 을 호출하는 스레드는 대상 스레드가 `TERMINATED` 상태가 될 때 까지 대기한다. 
> - 대상스레드가 `TERMINATED` 상태가 되면 호출 스레드는 다시 `RUNNABLE` 상태가 되면서 다음 코드를 수행한다.



- ### Join()의 단점으로는 다른 스레드가 완료되어질때 까지 무기한 기다리는 단점이 있습니다. 

- 그래서 특정 시간만큼만 대기할 수있도록 설정할 수있습니다.



``` java
package thread.control.join;

import static util.MyLogger.*;
import static util.ThreadUtils.*;

public class JoinMainV4 {

	public static void main(String[] args) throws InterruptedException {
		log("Start");
		SumTask task1 = new SumTask(1, 50);

		Thread thread1 = new Thread(task1, "thread-1");

		thread1.start();

		// 스레드가 종료될 때 까지 대기
		log("join() - main 스레드가 thread1 종료까지 1초 대기");
		// 이때 main 스레드의 상태는 Waiting이 아니라 timed_waiting이 된다.
		// 보통 무기한 대기하는 경우 waiting, 특정시간만큼 대기하면 timed-waiting이 됩니다.
		thread1.join(1000);
		log("main 스레드 대기 완료");

		log("task1.result = " + task1.result);
		log("End");

	}


	static class SumTask implements Runnable{

		int startValue;
		int endValue;
		int result = 0;

		public SumTask(int startValue, int endValue) {
			this.startValue = startValue;
			this.endValue = endValue;
		}

		@Override
		public void run() {
			log("작업시작");
			sleep(2000);
			int sum = 0;
			for(int i = startValue; i <= endValue;  i++){
				sum += i;
			}
			result = sum;
			log("작업 완료 result = " + result );
		}
	}
	
}

```



![image-20241223011555501](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20241223011555501.png)

- main 스레드는 join(1000)을 사용해서 thread-1을 1초간 기다립니다.
  - 이때 main 스레드의 상태는 waiting이 아니라 time_waiting이 됩니다.
  - 보통 무기한 대기하면 waiting 상태가 되고, 특정 시간만큼만 대기하는 경우 time_waiting 상태가 됩니다.
- thread-1` 의 작업에는 2초가 걸리고 main 스레드는 1초 대기 후 작업을 속행합니다.
  - 이때 main 스레드의 상태는 runnable입니다.
- main 스레드가 종료된 이후에 thread-1가 종료되어지고 계산결과가 나옵니다. 
