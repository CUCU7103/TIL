# Join - 특정시간 만큼 대기



join()은 두 가지 메서드가 존재합니다.

- join() 
  - 호출 스레드는 대상 스레드가 완료될 때 까지 무한정 대기합니다
- join(ms)
  - 호출 스레드는 특정 시간 만큼만 대기합니다. 
  - 호출 스레드는 지정한 시간이 지나면 다시 RUNNABLE 상태가 되면서 다음 코드를 수정합니다.



예제 코드

``` java
package thread.control.join;

import static util.MyLogger.log;
import static util.ThreadUtils.sleep;

public class JoinMainV4 {

    public static void main(String[] args) throws InterruptedException {
        log("Start");
        SumTask task1 = new SumTask(1, 50);
        Thread thread1 = new Thread(task1, "thread-1");

        thread1.start();

        // 스레드가 종료될 때 까지 대기
        log("join(1000) - main 스레드가 thread1 종료까지 1초 대기");
        thread1.join(1000);
        log("main 스레드 대기 완료");

        log("task1.result = " + task1.result);
        log("End");
    }

    static class SumTask implements Runnable {

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
            for (int i = startValue; i <= endValue; i++) {
                sum += i;
            }
            result = sum;
            log("작업 완료 result = " + result);
        }
    }
}

// 결과
02:05:16.072 [     main] join(1000) - main 스레드가 thread1 종료까지 1초 대기
02:05:16.072 [ thread-1] 작업 시작
02:05:17.073 [     main] main 스레드 대기 완료
02:05:17.073 [     main] task1.result = 0
02:05:17.073 [     main] End
02:05:18.076 [ thread-1] 작업 완료 result = 1275

```


![image](https://github.com/user-attachments/assets/1e77d71a-c811-4978-971d-9588048a5cfb)




- `main` 스레드는 `join(1000)` 을 사용해서 `thread-1` 을 1초간 기다린다.
  - 이때 `main` 스레드의 상태는 `WAITING` 이 아니라 `TIMED_WAITING` 이 된다.
    보통 무기한 대기하면 `WAITING` 상태가 되고, 특정 시간 만큼만 대기하는 경우 `TIMED_WAITING` 상태가
    된다.

- `thread-1` 의 작업에는 2초가 걸린다.
  - 1초가 지나도 `thread-1` 의 작업이 완료되지 않으므로, `main` 스레드는 대기를 중단합니다. 
  - 그리고 `main` 스레드는 다시 `RUNNABLE` 상태로 바뀌면서 다음 코드를 수행합니다..
  - 이때 `thread-1` 의 작업이 아직 완료되지 않았기 때문에 `task1.result = 0` 이 출력되어집니다.
  - `main` 스레드가 종료된 이후에 `thread-1` 이 계산을 끝낸다. 따라서 `작업 완료 result = 1275` 이 출력되어집니다.



### 정리

1. 다른 스레드가 끝날 때 까지 무한정 기다려야 한다면 `join()` 을 사용하고, 
2. 다른 스레드의 작업을 무한정 기다릴 수 없다면 `join(ms)` 를 사용하면 됩니다. 
3. 물론 기다리다 중간에 나오는 상황인데, 결과가 없다면 추가적인 오류 처리가 필요
   할 수 있습니다.
