# Synchronized 

> [!NOTE]
>
> ### **스레드 동기화**는 **<u>멀티스레드 환경에서 여러 스레드가 하나의 공유자원에 동시에 접근하지 못하도록 막는것을 말합니다.</u>** 
>
> #### 즉 Synchronized는여러 스레드에서 하나의 객체에 있는 인스턴스 변수를 동시에 처리해야 할때 발생하는 문제를 해결하기 위해 사용하는 것입니다.
>
> #### 공유데이터가 사용되어 <u>동기화가 필요한 부분을 임계영역(critical section)</u>이라고 부릅니다.
>
> #### 자바에서는 이 임계영역에 synchronized 키워드를 사용하여 여러 스레드가 동시에 접근하는 것을 금지함으로써 동기화를 할 수 있습니다. 
>
> #### 



Synchronized를 사용하지 않았을 때

```java
package thread;

public class CommonCalculate {

    private int amount;

    public CommonCalculate(){
        amount = 0;
    }

    public void plus (int value){
        amount += value;
    }

    public void minus(int value){
        amount -= value;
    }

    public int getAmount(){
        return amount;
    }
}

------------------------------------------------------------------
    
package thread;

public class ModifyAmountThread extends Thread{
    private CommonCalculate calc;
    private boolean addFlag;

    public ModifyAmountThread(CommonCalculate calc, boolean addFlag){
        this.calc = calc;
        this.addFlag = addFlag;
    }

    public void run (){
        for (int loop = 0; loop < 10000; loop++) {
            if(addFlag){
                calc.plus(1);
            }else{
                calc.minus(1);
            }
        }
    }
}
    
===================================================================================
public class RunSync {

    public static void main(String[] args) {
        RunSync runSync = new RunSync();
        runSync.runCommonCalculate();
    }

    private void runCommonCalculate() {
        CommonCalculate calc = new CommonCalculate();
        ModifyAmountThread thread1 = new ModifyAmountThread(calc,true);
        ModifyAmountThread thread2 = new ModifyAmountThread(calc, true);

        thread1.start();
        thread2.start();

        try{
            thread1.join();
            thread2.join();
            System.out.println("Final value is " + calc.getAmount());
        } catch(InterruptedException e){
            e.printStackTrace();
        }
    }
}
```

- 예상하기로는 결괏값이 20000이 나와야 하는데 실제로는 그렇지 않다 
  - 실제 나온값들 Final value is 12865,11609...
- 예상하지 못한 값들이 생성되어진다.
  - 즉  하나의 공유자원에 여러 스레드에서 동시에 접근하여 사용했기 때문입니다.



### Synchronized 사용방법

> [!NOTE]
>
> - #### synchronized 키워드는 **동기화가 필요한 메소드나 코드블럭앞에 사용하여 동기화 할 수 있습니다.**
>
> - #### **synchronized로 지정된 임계영역은 한 스레드가 이 영역에 접근하여 사용할때 lock이 걸림으로써 다른 스레드가 접근할 수 없게 됩니다.** 
>
> - #### 이후 해당 **스레드가 이 임계영역의 코드를 다 실행 후 벗어나게되면 unlock 상태가 되어 그때서야 대기하고 있던 다른 스레드가 이 임계영역에 접근하여 다시 lock을 걸고 사용할 수 있게 됩니다.** 
>
> - #### **synchronized로 설정된 임계영역은 lock 권한을 얻은 하나의 객체만이 독점적으로 사용하게됩니다.** 
>
>   - 즉 공유자원에 대한 작업은 하나의 스레드가 작업을 완료할때까지 다른 스레드들은 해당 공유자원에 접근이 불가하다고 생각하면된다.



1) #### **메소드에 synchronized 설정하기**

- 메소드 이름 앞에 synchronized 키워드를 사용하면 해당 메소드 전체를 임계영역으로 설정할 수 있습니다.. 

```java
synchronized void increase() {
	count++;
	System.out.println(count);
}

=======================================
    
public class CommonCalculate {

    private int amount;

    public CommonCalculate(){
        amount = 0;
    }

    // 스레드 동기화 사용
    public synchronized void plus (int value){
        amount += value;
    }
	// 스레드 동기화 사용 
    public synchronized void minus(int value){
        amount -= value;
    }

    public int getAmount(){
        return amount;
    }
}    

// 결과값은 우리가 예상하였던 20000이 나온다.
```



```java
 CommonCalculate calc = new CommonCalculate();
        ModifyAmountThread thread1 = new ModifyAmountThread(calc,true);
        ModifyAmountThread thread2 = new ModifyAmountThread(calc, true);
```

- 메서드에 sychronized 할 때에는 **같은 객체를 참조할 때에만 유효**합니다.



- 아래와 같이 두 개의 스레드가 각기 다른 객체를 참조한다면 syuchronized로 선언된 메서드는 같은 객체를 참조하는 것이 아니므로 사용하지 않는것과 동일합니다.

```java
CommonCalculate calc = new CommonCalculate();
ModifyAmountThread thread1 = new ModifyAmountThread(calc,true);
 
CommonCalculate calc = new CommonCalculate();
ModifyAmountThread thread2 = new ModifyAmountThread(calc, true);
```





2. #### **동기화 블럭을 만들어 동기화 시키는 방식**

   - 메서드의 일부분에서만 인스턴스 변수를 사용할 때 전체 메서드를 동기화 시키는건 필요없는 대기시간을 발생시키기에 해당 변수를 사용하는 곳에서만 동기화 되도록 동기화 블럭을 만들어서 사용한다.

```java
========================== this라고 사용하거나  
public void plus (int value){
        synchronized (this){
            amount += value;
        }
    }

    public void minus(int value){
       synchronized (this){
           amount -= value;
       }
    }

=========================== 객체를 생성해서 사용하기
Object lock = new Object();

public void plus (int value){
        synchronized (lock){
            amount += value;
        }
    }

    public void minus(int value){
       synchronized (lock){
           amount -= value;
       }
    }
```









