# Runnable을 만드는 다양한 방법

- 중첩 클래스를 사용하면 Runnable을 더 편리하게 만들 수 있다



- 정적 중첩 클래스를 사용함

``` java
package thread.start;

import static util.MyLogger.log;

public class InnerRunnableMainV1 {
    // 특정 클래스 안에서만 사용되는 경우는 정적 중첩 클래스를 사용하자
    // 스레드 생성
    public static void main(String[] args) {
        log("main start");

        MyRunnable runnable = new MyRunnable();
        Thread thread = new Thread(runnable);
        thread.start();

        log("main end");
    }

    static class MyRunnable implements Runnable {

        @Override
        public void run() {
            log("run()" );
        }
    }


}
```



- 익명클래스 사용

```java
package thread.start;

import static util.MyLogger.log;

public class InnerRunnableMainV2 {
    // 익명 특정 메서드 안에서만 간단하게 정의하고 사용하고 싶다면 익명클래스로 만드는 것이 좋다.
    public static void main(String[] args) {
        log("main start");

        // 익명 클래스
        Runnable runnable = new Runnable() {
            @Override
            public void run() {
                log("run()");
            }
        };
        Thread thread = new Thread(runnable);
        thread.start();

        log("main end");
    }




}
```

- 익명클래스 변수없이 직접 전달

```java
package thread.start;

import static util.MyLogger.log;

public class InnerRunnableMainV3 {
    // 익명 특정 메서드 안에서만 간단하게 정의하고 사용하고 싶다면 익명클래스로 만드는 것이 좋다.
    public static void main(String[] args) {
        log("main start");


        Thread thread = new Thread(new Runnable(){
            @Override
            public void run() {
                log("run()");
            }
        });
        thread.start();

        log("main end");
    }




}
```



- 람다식 사용

```java
package thread.start;

import static util.MyLogger.log;

public class InnerRunnableMainV4 {
    public static void main(String[] args) {
        log("main start");

        // 람다식 사용하기
        Thread thread = new Thread(() -> log("run()"));
        thread.start();

        log("main end");
    }




}
```





