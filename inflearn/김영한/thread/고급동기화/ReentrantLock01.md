# BLOCKED vs WAITING 

WATING 상태에 특정 시간까지만 대기하는 기능이 포함된 것이 TIMED_WAITING입니다.



### **인터럽트**

- `BLOCKED` 상태는 인터럽트가 걸려도 대기 상태를 빠져나오지 못한다. 여전히 `BLOCKED` 상태이다.
- `WAITING` , `TIMED_WAITING` 상태는 인터럽트가 걸리면 대기 상태를 빠져나온다. 그래서 `RUNNABLE` 상태로 변한다

**용도**

- `BLOCKED` 상태는 자바의 `synchronized` 에서 락을 획득하기 위해 대기할 때 사용된다.
- `WAITING` , `TIMED_WAITING` 상태는 스레드가 특정 조건이나 시간 동안 대기할 때 발생하는 상태이다.
- `WAITING` 상태는 다양한 상황에서 사용된다. 예를 들어, `Thread.join()` , `LockSupport.park()` ,
- `Object.wait()` 와 같은 메서드 호출 시 `WAITING` 상태가 된다.
- `TIMED_WAITING` 상태는 `Thread.sleep(ms),` `Object.wait(long timeout)` ,
- `Thread.join(long millis)` , `LockSupport.parkNanos(ns)` 등과 같은 시간 제한이 있는 대기 메서
  드를 호출할 때 발생한다.



![image-20250113232205541](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20250113232205541.png)



## ReetrantLock 이론

- 자바는 1.0부터 존재한 `synchronized` 와 `BLOCKED` 상태를 통한 통한 임계 영역 관리의 한계를 극복하기 위해 자바 1.5부터 `Lock` 인터페이스와 `ReentrantLock` 구현체를 제공합니다.

**synchronized 단점**

- **무한 대기**: `BLOCKED` 상태의 스레드는 락이 풀릴 때 까지 무한 대기한다.
  - 특정 시간까지만 대기하는 타임아웃X
  - 중간에 인터럽트X
    
    

- **공정성**: 
  - 락이 돌아왔을 때 `BLOCKED` 상태의 여러 스레드 중에 어떤 스레드가 락을 획득할 지 알 수 없다. 
  - 최악의 경우 특정 스레드가 너무 오랜기간 락을 획득하지 못할 수 있다.



### LockInterface

- Lock Interface는 동시성 프로그래밍에서 쓰이는 안전한 임계 영역을 위한 락을 구현하는데 사용되어집니다.
- Lock 인터페이스는 다음과 같은 메서드를 제공한다. 대표적인 구현체로 `ReentrantLock` 이 있다.

``` java
public interface Lock {
  void lock();
  void lockInterruptibly() throws InterruptedException;
  boolean tryLock();
  boolean tryLock(long time, TimeUnit unit) throws InterruptedException;
  void unlock();
  Condition newCondition();
}
```



![image-20250113233504039](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20250113233504039.png)

![image-20250113233511404](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20250113233511404.png)

![image-20250113233522069](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20250113233522069.png)

![image-20250113233535831](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20250113233535831.png)

![image-20250113233540569](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20250113233540569.png)

![image-20250113233550968](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20250113233550968.png)



- 위의 메서드들을 사용하면 고수준의 동기화 기법을 사용할 수 있습니다.
- Lock 인터페이스는 sychronized 블록보다 더 많은 유연성을 제공합니다.
  - 특히 lock을 특정 시간 만큼만 시도하거나, 인터럽트 가능한 락을 사용할때 유용합니다.
  - 다양한 메서드를 통해 sychronized의 단점인 무한 대기 문제도 깔끔하게 해결할 수있습니다.



Lock 인터페이스의  대표적인 구현체로 `ReentrantLock` 이 있는데, 이 클래스는 스레드가 공정하게 락을 얻을 수 있는 모드를 제공합니다.

``` java
import java.util.concurrent.locks.ReentrantLock;
public class ReentrantLockEx {
	// 비공정 모드 락
	private final Lock nonFairLock = new ReentrantLock();
	// 공정 모드 락
	private final Lock fairLock = new ReentrantLock(true);
	public void nonFairLockTest() {
		nonFairLock.lock();
		try {
			// 임계 영역
		} finally {
			nonFairLock.unlock();
		}
	}
	public void fairLockTest() {
		fairLock.lock();
		try {
			// 임계 영역
		} finally {
			fairLock.unlock();
		}
	}
}
```

### 비공정 모드

- ReentrantLock의 기본 모드입니다.
- 해당 모드에서는 락을 먼저 요청한 스레드가 락을 먼저 획득한다는 보장이 없습니다.
- 락을 풀었을때 대기 중인 스레드 중 아무나 락을 획득할 수있습니다.
- 이는 락을 빨리 획득할 수 있지만 특정 스레드가 장기간 락을 획득하지 못할 가능성도 있습니다.

### **비공정 모드 특징**

- **성능 우선**: 락을 획득하는 속도가 빠르다.
- **선점 가능**: 새로운 스레드가 기존 대기 스레드보다 먼저 락을 획득할 수 있다.
- **기아 현상 가능성**: 특정 스레드가 계속해서 락을 획득하지 못할 수 있다.



### 공정 모드 (Fair mode)

- 생성자에서 `true` 를 전달하면 된다. 예) `new ReentrantLock(true)`
- 공정 모드는 락을 요청한 순서대로 스레드가 락을 획득할 수 있게 한다. 이는 먼저 대기한 스레드가 먼저 락을 획득하게 되어 스레드 간의 공정성을 보장한다. 
- 그러나 이로 인해 성능이 저하될 수 있다.

### **공정 모드 특징**

- **공정성 보장**: 대기 큐에서 먼저 대기한 스레드가 락을 먼저 획득한다.
- **기아 현상 방지**: 모든 스레드가 언젠가 락을 획득할 수 있게 보장된다.
- **성능 저하**: 락을 획득하는 속도가 느려질 수 있다.

![image-20250113234939120](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20250113234939120.png)

