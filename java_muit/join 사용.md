# join 사용



```  java
package thread.control.join;

import static util.MyLogger.log;
import static util.ThreadUtils.sleep;

public class JoinMainV3 {

	public static void main(String[] args) throws InterruptedException {
		log("start");
		SumTask task1 = new SumTask(1,50);
		SumTask task2 = new SumTask(51,100);
		Thread thread1 = new Thread(task1,"thread-1");
		Thread thread2 = new Thread(task2,"thread-2");

		thread1.start();
		thread2.start();

		// 스레드가 종료되어질 때까지 대기
		log("join() - main 스레드가 thread1, thread2 종료까지 대기함.");
		thread1.join();
		thread2.join();
		log("main 스레드 대기완료");


		log("task1.result = " + task1.result );
		log("task2.result = " + task2.result );

		int sumAll = task1.result + task2.result;
		log("task1 + task2 = " + sumAll);
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
			log("작업 시작");
			sleep(2000);
			int sum = 0;
			for (int i = startValue; i <= endValue; i++){
				sum += i;
			}
			result = sum;
			log("작업 완료 result =" + result);
		}
	}

}

```



![image](https://github.com/user-attachments/assets/e6826d5a-acd7-448e-b66f-78befb43cfe6)


- main 스레드에서 다음 코드를 실행하게 되면 `main` 스레드는 `thread-1` , `thread-2` 가 종료될 때 까지 기다린다.
  - 이때 `main` 스레드는 `WAITING` 상태가 된다

```
thread1.join();
thread2.join();
```

- thread-1 이 아직 종료되지 않았다면  main 스레드는 thread1.join() 코드 안에서 더는 진행하지 않고 멈추어 기다린다. 
  - 이후에 `thread-1` 이 종료되면 `main` 스레드는 `RUNNABLE` 상태가 되고 다음 코드로 이동한다

- `thread-2` 이 아직 종료되지 않았다면 `main` 스레드는 `thread2.join()` 코드 안에서 진행하지 않고 멈추어  기다린다. 
  - 이후에 `thread-2` 이 종료되면 `main` 스레드는 `RUNNABLE` 상태가 되고 다음 코드로 이동한다.

위의 코드 예시의 경우 `thread-1` 이 종료되는 시점에 `thread-2` 도 거의 같이 종료되기 때문에 `thread2.join()` 은 대기하지 않고 바로 빠져나온다



Waiting (대기 상태)

- 스레드가 다른 스레드의 특정 작업이 완료되기를 무기한 기다리는 상태이다.
- `join()` 을 호출하는 스레드는 대상 스레드가 `TERMINATED` 상태가 될 때 까지 대기한다. 
- 대상 스레드가 `TERMINATED` 상태가 되면 호출 스레드는 다시 `RUNNABLE` 상태가 되면서 다음 코드를 수행한다.



특정 스레드가 완료될 때 까지 기다려야 하는 상황이라면 `join()` 을 사용하면 된다.





