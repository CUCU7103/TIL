# Fork/Join

- `Fork/Join Framework`는 `Java 7`에서 소개된 새로운 기능입니다.
- 병렬화 할 수 있는 작업을 재귀적으로 작은 작업으로 분할한 다음에 서브태스크 각각의 결과를 합쳐서 전체 결과를 만들도록 설계되었습니다.

#### <u>즉 하나의 작업을 작은 단위로 나눠서 여러 쓰레드가 동시에 처리하는 것을 쉽게 만들어 줍니다.</u>



**📌 ForkJoinPool**

- **ForkJoinTask**를 실행하는데 사용되는 스레드 풀로, 알고리즘 작업의 균형을 동적으로 조정하는 역할을 한다.

 

**📌 ForkJoinTask**

- 작업의 기본 단위이다. 
- 작업에는 **결과를 반환하지 않는 작업(RecursiveAction)**과 결과를 반환하는 작업(RecursiveTask)의 두 가지 형태가 있다.

 

**📌 Work-Stealing Algorithm**

- ForkJoinPool을 사용한 알고리즘의 마스코트가 되는 큰 특징 중 하나이다. 
- 말 그대로 스레드가 다른 작업의 작업을 “훔치”는 알고리즘이다. 
- 그럼으로써 모든 스레드가 유휴시간 없이 계속해서 작업을 처리할 수 있도록 한다.

![image](https://github.com/user-attachments/assets/78905b6d-85f2-4dfc-9b47-664f94668c6f)




#### fork() 호출시 발생되어지는 일

- 작업 분할
  - 작업은 작은 서브 태스크로 분할되어야 합니다.
  - 분할된 작업은 ForkJoinPool에 속한 스레드의 deque에 추가되어집니다.

- 비동기 작업입니다.
- 작업처리
  - 각 스레드는 자신의 deque에서 작업을 가져와서 처리합니다.
  - 작업 실행 중 다른 서브 태스크가 fork()를 통해 생성되어지면  이 서브 태스크는 같은 deque의 앞쪽에 추가되어지며 가장 최근에 추가된 작업부터 처리되어 집니다.

#### join() 호출 시 발생되어지는 일

- 작업 완료를 동기화 해줍니다.
  - 이전에 fork()한 다른 작업의 완료를 기다립니다
- 작업 훔치기 (work-stealing) 알고리즘 활용 :
  - 현재 스레드가 join()을 호출하고 대기상태에 들어가면 ForkJoinPool은 다른 스레드가 휴식 상태에 빠지지 않도록 해당 알고리즘을 활성화 합니다. 
  - 즉 대기 중인 스레드가 다른 스레드의 deque에서  실행할 수 있는 테스크를 찾아 실행합니다.
- 결과 합치기 
  - 모든 서브 태스크의 실행이 완료되어지고 그 결과가 준비되면 join()은 이 결과를 합병하여 최종 결과를 생성합니다.



### 흐름 요약

- 큰 작업이 ForkJoinPool에 제출되면, 이는 더 작은 하위 작업으로 나누어지며(Fork), 각 하위 작업은 병렬로 실행된다.

- 스레드가 작업을 완료하고 더 이상 할 일이 없을 때, 다른 스레드의 Deque에서 작업을 훔쳐서 실행(Work Stealing)한다.

- 모든 하위 작업이 완료되면, 그 결과는 최종 결과를 형성하기 위해 결합된다(Join).



#### Fork / Join 기능은 RecursiveAction과 RecurisiveTask라는 추상클래스를 사용해야 합니다.

- RecurisiveTask를 사용하려면 추상 메서드 compute를 구현해야 합니다.

```java
if (태스크가 충분히 작거나 더 이상 분할할 수 없으면) {
    순차적으로 태스크 계산(알고리즘)
} else {
    태스크를 두 서브태스크로 분할(재귀적 호출, Fork)
    모든 서브태스크의 연산이 완료될 때까지 기다림
    각 서브태스크의 결과를 합칩(Join)
}
```



![image-20241030232147761](C:\Users\syste\AppData\Roaming\Typora\typora-user-images\image-20241030232147761.png)



#### 예시

```java
package ForkandJoin;

import java.util.concurrent.RecursiveTask;

public class GetSum2 extends RecursiveTask<Long> {

    long from, to;

    public GetSum2(long from, long to) {
        this.from = from;
        this.to = to;
    }

    public Long compute() {
        long gap = to - from;
        try{
            Thread.sleep(1000);
        }catch (Exception e){
            e.printStackTrace();
        }

        log("From = " + from + " To= " + to);

        if (gap <= 3) { // 작업의 단위가 충분히 작을 경우
            // 해당 작업을 수행
            long tempSum = 0;
            for (long loop = from; loop <= to; loop++) {
                tempSum += loop;
            }
            log("Return !! " + from + " ~ " + to +" = " + tempSum);
            return tempSum;
        }

        long middle = (from + to) / 2; // 두 부분으로 나누어 작업을 시작하기 위해서 중간값을 찾는다.
        GetSum2 sumPre = new GetSum2(from, middle); // 작업 수행을 위한 객체 생성하기
        log("Pre From =" + from + " To=" + middle);
        sumPre.fork(); // 첫 번째 작업을 수행한다.

        GetSum2 sumPost = new GetSum2(middle + 1, to); // 작업 수행을 위한 객체 생성하기
        log("Post From =" + (middle + 1) + " To=" + to);
        return sumPost.compute() + sumPre.join();
        // compute()에서 나머지 작업을 실행하고  join에서 fork로 실행한 작업을 기다리고 작업이 완료되면 결과를 병합합니다. 
    }

    public void log(String message){
        String threadName = Thread.currentThread().getName();
        System.out.println("["+ threadName +"]" + message);
    }

}

===================================================================================
    
public class ForkJoinSample2 {
    static final ForkJoinPool mainPool = new ForkJoinPool();

    public static void main(String[] args) {
        ForkJoinSample2 sample = new ForkJoinSample2();
        sample.calculate();
    }

    private void calculate() {
        long from  = 0;
        long to = 10;

        GetSum2 sum = new GetSum2(from, to);
        Long result = mainPool.invoke(sum); // fork/join을 이용한 연산을 수행할때는 invork() 메서드를 호출해야합니다.
        System.out.println("Fork Join : Total sum of " + from + " ~ "+ to +" = " + result );

    }
}
      

// 결과
[ForkJoinPool-1-worker-1]From = 0 To= 10
[ForkJoinPool-1-worker-1]Pre From =0 To=5
[ForkJoinPool-1-worker-1]Post From =6 To=10
[ForkJoinPool-1-worker-1]From = 6 To= 10
[ForkJoinPool-1-worker-2]From = 0 To= 5
[ForkJoinPool-1-worker-1]Pre From =6 To=8
[ForkJoinPool-1-worker-2]Pre From =0 To=2
[ForkJoinPool-1-worker-1]Post From =9 To=10
[ForkJoinPool-1-worker-2]Post From =3 To=5
[ForkJoinPool-1-worker-1]From = 9 To= 10
[ForkJoinPool-1-worker-2]From = 3 To= 5
[ForkJoinPool-1-worker-3]From = 6 To= 8
[ForkJoinPool-1-worker-4]From = 0 To= 2
[ForkJoinPool-1-worker-4]Return !! 0 ~ 2 = 3
[ForkJoinPool-1-worker-2]Return !! 3 ~ 5 = 12
[ForkJoinPool-1-worker-3]Return !! 6 ~ 8 = 21
[ForkJoinPool-1-worker-1]Return !! 9 ~ 10 = 19
Fork Join : Total sum of 0 ~ 10 = 55
```

- Fork/join의 결과를 확인하려면 ForkJoinPool의 invork() 메서드에 계산을 수행하는 객체를 넘겨주어야 합니다.
- 넘겨주는 순간 작업이 수행어집니다.



여기서 알 수있는 점은 Thread 객체를 만들지도 않았고, Thread 작업을 할당하지도 않았다.

해당 작업은 jvm에서 자동으로 수행해준다.





