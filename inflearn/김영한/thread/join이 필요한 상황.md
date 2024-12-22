# Join이 필요한 상황

- main` 스레드가 `1 ~ 100` 으로 더하는 작업을 `thread-1` , `thread-2` 에 각각 작업을 나누어 지시하면 CPU 코어를 더 효율적으로 활용할 수 있다. 

- CPU 코어가 2개라면 이론적으로 연산 속도가 2배 빨라집니다.

- 그렇다면 아래와 같이 생각해볼 수 있습니다.

  - thread-1 : 1 ~ 50 까지 더하기

  - thread-2 : 51 ~ 100 까지 더하기

  - main : 두 스레드의 계산 결과를 받아서 합치기

``` java
package thread.control.join;

import static util.MyLogger.*;
import static util.ThreadUtils.*;

public class JoinMainV1 {

	public static void main(String[] args) {
		log("Start");
		SumTask task1 = new SumTask(1, 50);
		SumTask task2 = new SumTask(51, 100);

		Thread thread1 = new Thread(task1, "thread-1");
		Thread thread2 = new Thread(task2, "thread-2");

		thread1.start();
		thread2.start();


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
	 * 18:15:58.267 [     main] Start
	 * 18:15:58.273 [ thread-2] 작업시작
	 * 18:15:58.273 [ thread-1] 작업시작
	 * 18:15:58.279 [     main] task1.result = 0
	 * 18:15:58.280 [     main] task2.result = 0
	 * 18:15:58.281 [     main] sumAll = 0
	 * 18:15:58.281 [     main] End
	 * 18:16:00.277 [ thread-2] 작업 완료 result = 3775
	 * 18:16:00.287 [ thread-1] 작업 완료 result = 1275
	 */

}

```

- 위와 같이 코드를 구성하고 실행하였으나 예상한 결과가 나오지 않습니다.

- 실행 결과는 task1과 task2의 결과가 모두 0으로 나옵니다.

  - 즉 계산이 전혀 진행되지 않았습니다.

    

그 이유는 다음과 같습니다.

![image-20241222184222734](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20241222184222734.png)

- main 스레드는 theard-1 과 2에 작업을 지시하고  각 스레드가 작업을 완료하기도 전에 먼저 계산 결과를 조회했습니다.
- 그래서 task1 + task2 = 0으로 나옵니다.



![image-20241222184442592](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20241222184442592.png)

- 프로그램이 처음 시작되면 `main` 스레드는 `thread-1` , `thread-2` 를 생성하고 `start()` 로 실행한다.
  - **스레드 객체를 생성하고 반드시 start()를 호출해야 스택공간을 할당받고 스레드가 동작합니다.** 
- thread-1, thread-2 는 **각 자신에게 전달된 SumTask 인스턴스의 run() 메서드를 스택에 올리고 실행**한다.
- thread-1 은 x001 인스턴스의 run() 메서드를 실행한다.
- thread-2 는 x002 인스턴스의 run() 메서드를 실행한다
- main 스레드는 두 스레드를 시작한 다음에 바로 task1.result , task2.result를 통해 인스턴스에 있는결과 값을 조회한다.
- 참고로 main 스레드가 실행한 start() 메서드는 스레드의 실행이 끝날 때 까지 기다리지 않는다! 다른 스레드를 실행만 해두고, 자신의 다음 코드를 실행할 뿐입니다.
- thread-1 , thread-2 가 계산을 완료해서, result 에 연산 결과를 담을 때 까지는 약 2초 정도의 시간이 걸린다.
- main 스레드는 계산이 끝나기 전에 result 의 결과를 조회한 것이다. 
- 따라서 0 값이 출력된다.



![image-20241222185028744](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20241222185028744.png)

- 2초가 지난 이후에 thread-1, thread-2는 계산을 완료합니다.

- 이때 main 스레드는 이미 자신의 코드를 모두 실행하고 종료된 상태입니다.

- task1 인스턴스의 result에는 1275가 담겨져 있고 task2 인스턴스의 result에는 3775가 담겨져 있습니다.

  

### 참고 - this의 비밀

어떤 메서드를 호출하는 것은, 정확히는 특정 스레드가 어떤 메서드를 호출하는 것이다.

- **스레드는 메서드의 호출을 관리하기 위해 메서드 단위로 스택 프레임을 만들고 해당 스택 프레임을 스택위에 쌓아 올린다**.

**이때 인스턴스의 메서드를 호출하면, 어떤 인스턴스의 메서드를 호출했는지 기억하기 위해, 해당 인스턴스의 참조값을 스택 프레임 내부에 저장**해둔다. 

- 즉 스택 프레임 내부에 인스턴스의 참조값을 this가 가지고 있는것이다.

이것이 바로 우리가 자주 사용하던 `this` 이다.

특정 메서드 안에서 `this` 를 호출하면 바로 스택프레임 안에 있는 `this` 값을 불러서 사용하게 된다.

그림을 보면 스택 프레임 안에 있는 `this` 를 확인할 수 있다.

이렇게 `this` 가 있기 때문에 `thread-1` , `thread-2` 는 자신의 인스턴스를 구분해서 사용할 수 있다. 

예를 들어서 필드에 접근할 때 `this` 를 생략하면 자동으로 `this` 를 참고해서 필드에 접근한다.
정리하면 `this` 는 호출된 인스턴스 메서드가 소속된 객체를 가리키는 참조이며, 이것이 스택 프레임 내부에 저장되어있다.
