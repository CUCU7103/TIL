# volatile

### 메모리 가시성 문제

```java
package thread.volatile1;

import static util.MyLogger.*;
import static util.ThreadUtils.*;

public class VolatileFlagMain {

	public static void main(String[] args) {
		MyTask task = new MyTask();
		Thread t = new Thread(task,"work");
		log("runFlag = " + task.runFlag);
		t.start();
		sleep(1000);

		log("runFlag를 false로 변경 시도");
		task.runFlag = false;
		log("runFlag = " + task.runFlag);
		log("main 종료");
	}

	static class MyTask implements Runnable{
		// boolean runflag = true;
		volatile boolean runFlag = true;

		@Override
		public void run() {
			log("task 시작");
			while(runFlag){
				// runflag가 fasle로 변하면 탈출
			}

			log("task 종료");
		}
	}


}

```

![image-20250101204649174](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20250101204649174.png)

- 점선 위쪽은 스레드의 실행 흐름을 나타내고, 점선 아래쪽은 하드웨어를 나타낸다.
- 자바 프로그램을 실행하고 `main` 스레드와 `work` 스레드는 모두 `runFlag` 의 값을 읽는다.
- CPU는 이 값을 효율적으로 처리하기 위해 먼저 캐시 메모리에 불러온다.
- `main` 스레드와 `work` 스레드가 사용하는 `runFlag` 가 각각의 캐시 메모리에 보관된다.
- 프로그램의 시작 시점에는 `runFlag` 를 변경하지 않기 때문에 모든 스레드에서 `true` 의 값을 읽는다.
- 참고로 `runFlag` 의 초기값은 `true` 이다.
- `work` 스레드의 경우 `while(runFlag[true])` 가 만족하기 때문에 while문을 계속 반복해서 수행한다



![image-20250101204741366](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20250101204741366.png)

- main 스레드는 runflag를 false로 설정한다.
- **<u>이때 캐시 메모리의 runflag가 false로 설정되어진다.</u>**

![image-20250101204926167](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20250101204926167.png)



![image-20250101205045123](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20250101205045123.png)

- 메인 메모리에 변경된 `runFlag` 값이 언제 CPU 코어2의 캐시 메모리에 반영될까?
- 이 부분에 대한 정답도 "알 수 없다"이다. CPU 설계 방식과 종류의 따라 다르다. 극단적으로 보면 평생 반영되지 않을 수도 있다!
- 언젠가 CPU 코어2의 캐시 메모리에 `runFlag` 값을 불러오게 되면 `work` 스레드가 확인하는 `runFlag` 의 값이 `false` 가 되므로 while문을 탈출하고, `"task 종료"` 를 출력한다

- 캐시 메모리를 메인 메모리에 반영하거나, 메인 메모리의 변경 내역을 캐시 메모리에 다시 불러오는 것은 언제 발생할까?
- 이 부분은 CPU 설계 방식과 실행 환경에 따라 다를 수 있다. 즉시 반영될 수도 있고, 몇 밀리초 후에 될 수도 있고, 몇 초후에 될 수도 있고, 평생 반영되지 않을 수도 있다.
- 주로 컨텍스트 스위칭이 될 때, 캐시 메모리도 함께 갱신되는데, 이 부분도 환경에 따라 달라질 수 있다.
- 예를 들어 `Thread.sleep()` 이나 콘솔에 내용을 출력할 때 스레드가 잠시 쉬는데, 이럴 때 컨텍스트 스위칭이 되면서 주로 갱신된다. 하지만 이것이 갱신을 보장하는 것은 아니다.



### 이처럼 멀티스레드 환경에서 한 스레드가 변경한 값이 다른 스레드에서 언제 보이는지에 대한 문제를 **<u>메모리 가시성 문제</u>**라고 합니다.



- 이러한 메모리 가시성 문제를 해결하기 위해 volatile 키워드를 적용합니다.

![image-20250101210619031](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20250101210619031.png)

- 캐시 메모리에 저장하지 않고 메인 메모리에 저장하기 때문에 성능이 좀 느려집니다.



**메모리 가시성(memory visibility)**

- 캐시 메모리를 메인 메모리에 반영하거나, 메인 메모리의 변경 내역을 캐시 메모리에 다시 불러오는 것은 언제 발생할까?
- 이 부분은 CPU 설계 방식과 실행 환경에 따라 다를 수 있다. 즉시 반영될 수도 있고, 몇 밀리초 후에 될 수도 있고, 몇 초후에 될 수도 있고, 평생 반영되지 않을 수도 있다.
- 주로 컨텍스트 스위칭이 될 때, 캐시 메모리도 함께 갱신되는데, 이 부분도 환경에 따라 달라질 수 있다.
  - `Thread.sleep()` , 콘솔에 출력등을 할 때 스레드가 잠시 쉬는데, 이럴 때 **컨텍스트 스위칭이 되면서 주로 갱신**된다. 
  - 하지만 이것이 갱신을 보장하는 것은 아니다
