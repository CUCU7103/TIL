# join이 필요한 상황



- 스레드 2개로 1 부터 50까지 의 수를 더해보자



`main` 스레드가 `1 ~ 100` 으로 더하는 작업을 `thread-1` , `thread-2` 에 각각 작업을 나누어 지시하면 CPU 코어를 더 효율적으로 활용할 수 있습니다.

 CPU 코어가 2개라면 이론적으로 연산 속도가 2배 빨라진다.
`thread-1` : `1 ~ 50` 까지 더하기
`thread-2` : `51 ~ 100` 까지 더하기
`main` : 두 스레드의 계산 결과를 받아서 합치기





```java
package thread.control.join;

import static util.MyLogger.log;
import static util.ThreadUtils.sleep;

public class JoinMainV1 {

	public static void main(String[] args) {
		log("start");
		SumTask task1 = new SumTask(1,50);
		SumTask task2 = new SumTask(51,100);
		Thread thread1 = new Thread(task1,"thread-1");
		Thread thread2 = new Thread(task2,"thread-2");

		thread1.start();
		thread2.start();

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


// 결과

01:36:19.514 [     main] start
01:36:19.517 [ thread-2] 작업 시작
01:36:19.517 [ thread-1] 작업 시작
01:36:19.519 [     main] task1.result = 0
01:36:19.520 [     main] task2.result = 0
01:36:19.520 [     main] task1 + task2 = 0
01:36:19.520 [     main] End
01:36:21.525 [ thread-2] 작업 완료 result =3775
01:36:21.525 [ thread-1] 작업 완료 result =1275



```



하지만 실행결과는 예상한 대로 나오지 않았습니다.

![image](https://github.com/user-attachments/assets/271637c7-7c6b-4ae0-abb9-fe818651be95)


- main thread가 thread-1, 2가 연산을 끝내기도 전에 결과를 조회하였고 그 결과 0이 나왔습니다.



- 해당 부분을 메모리 구조로 살펴보자
- 
![image](https://github.com/user-attachments/assets/e7e71589-261c-4a3f-81a8-8d63a41443ee)


- 프로그램이 처음 시작되면 `main` 스레드는 `thread-1` , `thread-2` 를 생성하고 `start()` 로 실행한다.
- `thread-1` , `thread-2` 는 각각 자신에게 전달된 `SumTask` 인스턴스의 `run()` 메서드를 스택에 올리고 실행한다.
  - `thread-1` 은 `x001` 인스턴스의 `run()` 메서드를 실행한다.
  - `thread-2` 는 `x002` 인스턴스의 `run()` 메서드를 실행한다



![image](https://github.com/user-attachments/assets/e999002d-88b3-4234-9e7d-a5fb4be8d669)


- `main` 스레드는 두 스레드를 시작한 다음에 바로 `task1.result` , `task2.result` 를 통해 인스턴스에 있는 결과 값을 조회한다. 
- 참고로 `main` 스레드가 실행한 `start()` 메서드는 스레드의 실행이 끝날 때 까지 기다리지 않는다! 다른 스레드를 실행만 해두고, 자신의 다음 코드를 실행할 뿐이다!
- `thread-1` , `thread-2` 가 계산을 완료해서, `result` 에 연산 결과를 담을 때 까지는 약 2초 정도의 시간이 걸린다.
-  `main` 스레드는 계산이 끝나기 전에 `result` 의 결과를 조회한 것이다. 따라서 `0` 값이 출력된다.



![image](https://github.com/user-attachments/assets/c6fe9dc1-8180-4872-820d-9a77a55a0314)


- 2초가 지난 이후에 `thread-1` , `thread-2` 는 계산을 완료한다.
- 이때 `main` 스레드는 이미 자신의 코드를 모두 실행하고 종료된 상태이다.
- `task1` 인스턴스의 `result` 에는 `1275` 가 담겨있고, `task2` 인스턴스의 `result` 에는 `3775` 가 담겨있다



#### main 스레드가 저 2개의 스레드의 연산이 끝날때 까지 기다리게 하려면 어떤 방법이 있을까

- 특정 스레드를 기다리게 하는 가장 간단한 방법은 `sleep()` 을 사용하는 것



``` java
package thread.control.join;

import static util.MyLogger.log;
import static util.ThreadUtils.sleep;

public class JoinMainV2 {

	public static void main(String[] args) {
		log("start");
		SumTask task1 = new SumTask(1,50);
		SumTask task2 = new SumTask(51,100);
		Thread thread1 = new Thread(task1,"thread-1");
		Thread thread2 = new Thread(task2,"thread-2");

		thread1.start();
		thread2.start();

		log("main thread sleep()");
		sleep(3000);
		log("main 스레드 깨어남");

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

// 
01:41:47.870 [     main] start
01:41:47.872 [     main] main thread sleep()
01:41:47.872 [ thread-2] 작업 시작
01:41:47.872 [ thread-1] 작업 시작
01:41:49.883 [ thread-1] 작업 완료 result =1275
01:41:49.883 [ thread-2] 작업 완료 result =3775
01:41:50.880 [     main] main 스레드 깨어남
01:41:50.881 [     main] task1.result = 1275
01:41:50.881 [     main] task2.result = 3775
01:41:50.881 [     main] task1 + task2 = 5050
01:41:50.883 [     main] End

```



![image](https://github.com/user-attachments/assets/dcdde7d0-274d-40e4-870c-1f91e68dfb97)


`thread-1`,`thread-2` 는 계산에 2초 정도의 시간이 걸린다. 

우리는 이 부분을 알고 있어서 `main` 스레드가 약 3초 후에 계산 결과를 조회하도록 했다. 따라서 계산된 결과를 받아서 출력할 수 있습니다.



하지만 이렇게 `sleep()` 을 사용해서 무작정? 기다리는 방법은 대기 시간에 손해도 보고, 또 `thread-1` ,
`thread-2` 의 수행시간이 달라지는 경우에는 정확한 타이밍을 맞추기 어렵습니다.



이에 대한 해결책으로 join()이 있습니다























