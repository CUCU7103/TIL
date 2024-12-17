# 스레드 생성 - Runnable

- 스레드를 만들때는 Thread 클래스를 상속받는 방법과 Runnable 인터페이스를 구현하는 방법이 있다.

Runnable 인터페이스를 구현하는 방법

```  java
public class HelloRunnable implements Runnable {
	@Override
	public void run() {
		System.out.println(Thread.currentThread().getName() + ": run()");
	}
}
```

```  java
public class HelloRunnableMain {
	public static void main(String[] args) {
		System.out.println(Thread.currentThread().getName() + " : main() start");

		HelloRunnable runnable = new HelloRunnable();
		Thread thread = new Thread(runnable); // Runnable을 받는다.
		thread.start();

		System.out.println(Thread.currentThread().getName() + " : main() end");
	}
}
```



### 그렇다면 실무에서는 어떤 방법을 사용해야 할까?

- 스레드를 사용할 때는 Thread를 상속받는 방법보다 Runnable 인터페이스를 구현하는 방식을 사용하자.

- Thread 클래스 상속방식은 Runnable 인터페이스 구현 방식보다 간단하지만 다른 클래스를 상속받고 있는 경우 Thread 클래스를 상속받을 수없다.
- 그리고 Runnable 인터페이스에서는 run 메서드만 오버라이딩 하면되지만 Thread 클래스를 상속받으면 사용에 불필요한 메서드까지 사용할 수있어 문제가 
