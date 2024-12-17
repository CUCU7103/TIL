# Runnable을 만드는 다양한 방법

- 중첩 클래스 사용
  - 특정 클래스 안에서만 사용되는 경우 중첩 클래스를 사용하면 됩니다.

```  java
public class InnerRunnableMainV1 {
	public static void main(String[] args) {
		log("main() start");

		MyRunnable runnable = new MyRunnable();
		Thread thread = new Thread(runnable);
		thread.start();

		log("main() end");
	}
	// 정적 중첩 클래스 사용
	static class MyRunnable implements Runnable {

		@Override
		public void run() {
			log(Thread.currentThread().getName() + ": run()");
		}

	}
}

```



- 익명 클래스 사용
  - 특정 메서드 안에서만 간단히 정의하고 사용하고 싶다면 익명 클래스를 사용하면 됩니다.

``` java
public class InnerRunnableMainV2 {
	public static void main(String[] args) {
		log("main() start");

		// 익명클래스를 사용
		// 특정 메서드 안에서만 간단하게 사용하고 싶다면 익명 클래스를 사용하면 된다.
		Runnable runnable = new Runnable() {
			@Override
			public void run() {
				log("run()");
			}
		};
		Thread thread = new Thread(runnable);
		thread.start();

		log("main() end");
	}
}
```



- 익명 클래스 변수없이 직접 전달

```java
public class InnerRunnableMainV3 {
	public static void main(String[] args) {
		log("main() start");

		// 익명클래스를 변수없이 직접 전달하는 방법이 있다.
		// 변수를 합칠 수도 있다.
		Thread thread = new Thread(new Runnable() {
			@Override
			public void run() {
				log("run()");
			}
		});
		thread.start();

		log("main() end");
	}
}
```



- 람다식 사용

``` java
public class InnerRunnableMainV4 {
	public static void main(String[] args) {
		log("main() start");

		// 람다식 사용.
		Thread thread = new Thread(() -> log("run()"));
		thread.start();

		log("main() end");
	}
}
```







