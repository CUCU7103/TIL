# sychronized 코드블럭

- sychronized의 가장 큰 장점이자 단점은 한 번에 하나의 스레드만 실행할 수 있다는 점입니다.
- 여러 스레드가 동시에 실행하지 못하기 때문에 전체적으로 보면 성능이 떨어질 수 있습니다.

- 따라서 synchronized를 통해 여러 스레드를 동시에 실행할 수 없는 코드 구간은 반드시 필요한 곳으로 한정해서 사용해야 합니다.
- 자바는 이러한 문제를 해결하기 위해 sychronized를 메서드 단위가 아니라 특정 코드 블럭에 최적화해서 적용할 수 있는 기능을 제공합니다.

```  java
package thread.sync;

import static util.MyLogger.*;
import static util.ThreadUtils.*;

public class BackAccountV3 implements BankAccount {

	volatile private int balance;

	public BackAccountV3(int initialBalance) {
		this.balance = initialBalance;
	}


	@Override
	public boolean withdraw(int amount) { // 동기화 되었다. 한 번에 하나의 스레드만 실행할 수있습니다.
		log("거래 시작" + getClass().getSimpleName()); // 클래스 이름 찍기
		// 잔고가 출금액 보다 적으면 , 진행하면 안됨
		// === 임계영역 시작
		synchronized (this){ // 여기서 락을 획득함.
			log("[검증 시작] 출금액: " + amount + ", 잔액 : " + balance);
			if(balance < amount){
				log("[검증 실패] 출금액: " + amount + ", 잔액 : " + balance);
				return false;
			}
			// 잔고가 출금액 보다 많으면, 진행
			log("[검증 완료] 출금액 : " + amount + ", 잔액 :" + balance);
			sleep(1000); // 출금에 걸리는 시간으로 가정
			balance = balance - amount;
			log("[출금 완료] 출금액 : " + amount + ", 잔액 :" + balance);
		}
		// === 임계영역 종료
		log("거래 종료"); // 클래스 이름 찍기
		return true;
	}

	@Override
	public synchronized int getBalance() { // 동기화 되었다. 한 번에 하나의 스레드만 실행할 수있습니다.
		return balance;
	}
}
```

- withdraw()` 메서드 앞에 사용하던 `synchronized` 를 제거한다.
  `
- synchronized (this) {}` : 안전한 임계 영역을 코드 블럭으로 지정한다.` 
- 이렇게 하면 꼭 필요한 코드만 안전한 임계 영역으로 만들 수 있다.
- `synchronized (this)` : 여기서 괄호 `()` 안에 들어가는 값은 락을 획득할 인스턴스의 참조이다.
- 여기서는 `BankAccountV3(x001)` 의 인스턴스의 락을 사용하므로 이 인스턴스의 참조인 `this` 를 넣어주면 된다.
- 이전에 메서드에 `synchronized` 를 사용할 때와 같은 인스턴스에서 락을 획득한다.
- `getBalance()` 의 경우 `return balance` 코드 한 줄이므로 `synchronized` 를 메서드에 설정하나 코드블럭으로 설정하나 둘다 같다



### 실행결과

```
22:24:30.870 [       t2] 거래 시작BackAccountV3
22:24:30.870 [       t1] 거래 시작BackAccountV3
22:24:30.877 [       t2] [검증 시작] 출금액: 800, 잔액 : 1000
22:24:30.877 [       t2] [검증 완료] 출금액 : 800, 잔액 :1000
22:24:31.359 [     main] t1 state: BLOCKED
22:24:31.359 [     main] t2 state: TIMED_WAITING
22:24:31.878 [       t2] [출금 완료] 출금액 : 800, 잔액 :200
22:24:31.878 [       t2] 거래 종료
22:24:31.878 [       t1] [검증 시작] 출금액: 800, 잔액 : 200
22:24:31.878 [       t1] [검증 실패] 출금액: 800, 잔액 : 200
22:24:31.879 [     main] 최종 잔액 : 200ㅁ
```



![image-20250105222458253](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20250105222458253.png)



