# Blocking  VS non Blocking 

### 처리 되어야 하는  작업이 전체 작업을 막는지에 대한 흐름입니다.

### 즉  <u>다른 요청의 작업을 처리하기 위해 현재 작업을 block(차단, 대기) 하냐 안하냐의 유무를 나타내는 프로세스의 실행 방식</u>이라고 할 수 있습니다.

### 조금 더 간단하게 말하자면 **<u>작업을 요청한 후 다른 작업을 수행할 수 있는지 여부에 의한 차이</u>**입니다.

### 

![image](https://github.com/user-attachments/assets/8e1e83e6-546f-433f-9bf6-882fb78206fe)


- ### 동기, 비동기가 전체적인 작업에 대한 순차적인 흐름의 유무라고하면 blocking / non-blocking은 전체적인 작업의 흐름 자체를 막는건지 막지 않는 건지로 볼 수있습니다.



> [!NOTE]
>
> - 제어권은 프로그램 실행 중 함수나 스레드가 자신의 코드를 실행할 수 있는 권한을 의미합니다.



## blocking

#### 작업을 요청한 후 해당 작업이 완료될 때까지 프로세스나, 스레드,호출한 함수가 대기 상태에 머무릅니다. 

#### 이 경우 제어권이 작업이 완료될 때까지 호출된 측에 머무르며, 호출한 측은 다른 작업을 수행하지 못하고 대기해야 합니다.

- 작업이 완료되기 전까지 프로세스의 실행이 멈추기 때문에, 요청을 보내고 그 결과를 반환받기 전까지는 다른 작업을 수행하지 못합니다.
- 즉 작업을 수행하는 동안 CPU가 다른 작업을 하지 못하고 멈춰 있게 됩니다.
  -  이로 인해 작업이 끝날 때까지 프로그램의 실행 흐름이 일시 중지됩니다.

- 예를 들어, 파일을 읽거나 네트워크 요청을 보낸 후 응답이 올 때까지 대기하고 있는 상황이 블록입니다.

![image](https://github.com/user-attachments/assets/2dd752b5-aa01-45b1-b049-2944ac5314dc)


## non-blocking

#### 작업을 요청한 후 즉시 제어권을 반환받아 다른 작업을 수행할 수 있습니다. 

#### 호출한 측은 작업의 완료 여부와 상관없이 다른 작업을 계속 수행할 수 있으며, 

#### 작업 완료 시 콜백 함수나 이벤트를 통해 결과를 처리합니다

- 즉, 요청을 보낸 후에 작업이 완료될 때까지 기다리지 않고, 다른 코드를 실행하면서 나중에 결과를 받습니다.

- 작업을 요청한 후에도 CPU가 다른 작업을 수행할 수 있습니다.
  -  작업이 완료되면, 결과를 처리하기 위한 콜백이나 이벤트가 트리거됩니다

- 예를 들어, 네트워크 요청을 보낸 후 그 결과를 기다리지 않고 다른 작업을 처리하고, 결과가 준비되면 그때 처리하는 방식이 논블록입니다.

![image](https://github.com/user-attachments/assets/e5e21efb-84ef-4b9e-ae83-87cc5b76ba49)








## **동기/비동기 + 블로킹/논블로킹 조합**

![image](https://github.com/user-attachments/assets/2755de02-ef06-4b95-8fe9-1d5503dd2c43)




| **조합**              | **설명**                                                     | **예시**                                                     |
| --------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **동기 + 블로킹**     | 작업을 요청하면 해당 작업이 완료될 때까지 대기하며, 완료 후 다음 작업을 수행합니다. | 파일을 읽는 함수 호출 시, 파일 읽기가 완료될 때까지 프로그램이 대기하는 경우. |
| **동기 + 논블로킹**   | 작업을 요청하면 즉시 제어권을 반환받아 다른 작업을 수행할 수 있지만, 작업의 완료 여부를 호출한 측에서 주기적으로 확인해야 합니다. | 논블로킹 소켓에서 데이터를 읽을 때, 데이터가 준비되지 않았더라도 즉시 반환하여 다른 작업을 수행하고, 이후 주기적으로 데이터 수신 여부를 확인하는 경우 |
| **비동기 + 블로킹**   | 작업을 요청하면 해당 작업이 완료될 때까지 대기하지만, 작업 완료 시 호출된 측에서 결과를 처리합니다. | 비동기 함수 호출 시, 작업이 완료될 때까지 대기하고, 완료되면 호출된 함수에서 콜백 함수를 통해 결과를 처리하는 경우. |
| **비동기 + 논블로킹** | 작업을 요청하면 즉시 제어권을 반환받아 다른 작업을 수행할 수 있으며, 작업 완료 시 호출된 측에서 결과를 처리합니다. | 네트워크 요청을 비동기적으로 보내고, 응답을 기다리지 않고 다른 작업을 수행하며, 응답이 도착하면 콜백 함수를 통해 결과를 처리하는 경우. |
|                       |                                                              |                                                              |



## Sync + blocking

- **<u>다른 작업이 진행되는 동한 자신의 작업을 처리하지 않고 , 다른 작업의 완료 여부를 바로 받아 순차적으로 처리하는 방식입니다.</u>**
- 다른 작업의 결과가 자신의 작업에 영향을 주는 경우에 사용할 수 있습니다.

![image](https://github.com/user-attachments/assets/2eeb92b8-4983-4298-88fe-b65b9dca2637)


- 예시

  - 파일을 읽어 내용을 처리하는 로직

    ``` java
    import java.io.BufferedReader;
    import java.io.FileReader;
    import java.io.IOException;
    
    public class SyncBlockingExample {
        public static void main(String[] args) {
            String filePath = "example.txt"; // 읽을 파일 경로
    
            try (BufferedReader reader = new BufferedReader(new FileReader(filePath))) {
                String line;
                while ((line = reader.readLine()) != null) {
                    System.out.println(line);
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
    
            System.out.println("파일 읽기 완료 후 다음 작업 수행");
        }
    }
    ```

    

  ## Async Non-blocking 

  - **<u>다른 작업이 진행되는 동안에도 자신의 작업을 처리하고 (Non Blocking), 다른 작업의 결과를 바로 처리하지 않아 작업 순서가 지켜지지 않는 (Async) 방식입니다.</u>**

  - 다른 작업의 결과가 자신의 작업에 영향을 주지 않은 경우에 활용

    

![image](https://github.com/user-attachments/assets/1cce8e32-2620-4ca1-914f-6b2e5a80976e)

- 예시
  - 웹 브라우저의 파일 다운로드가 비동기 논블로킹을 활용하는 예시 라고 할 수 있습니다.
  - 웹 브라우저는 웹 사이트에서 파일을 다운로드할 때, 파일의 전송이 완료될 때까지 다른 작업을 하지 않고 기다리는 것이 아니라, 다른 탭이나 창을 열거나 웹 서핑을 할 수 있습니다.
  -  이는 웹 브라우저가 파일 다운로드를 비동기적으로 처리하고, 콜백 함수를 통해 다운로드가 완료되면 알려주는 방식으로 구현되어 있기 때문이다

```  java
// 비동기적으로 파일 읽기
const fs = require('fs'); // 파일 시스템 모듈 불러오기

fs.readFile('file.txt', 'utf8', (err, data) => { // 파일 읽기 요청과 콜백 함수 전달
  if (err) throw err; // 에러 처리
  console.log(data); // 파일 내용 출력
});

fs.readFile('file2.txt', 'utf8', (err, data) => {
  if (err) throw err; 
  console.log(data);
});

fs.readFile('file3.txt', 'utf8', (err, data) => { 
  if (err) throw err; 
  console.log(data);
});

console.log('done'); // 작업 완료 메시지 출력

```

- Async Non Blocking은 작업량이 많거나 시간이 오래 걸리는 작업을 처리해야 하는 경우에 적합합니다.

- 대용량 데이터를 처리하거나 많은 요청을 처리하는 서비스에서는 Async Non Blocking 방식을 사용하여 한 작업이 처리되는 동안 다른 작업을 처리할 수 있으므로 전체 처리 시간을 줄일 수 있어 어플리케이션 처리 성능을 향상시킬 수 있게 됩니다.

  

## **Sync Non-Blocking**

- **<u>다른 작업이 진행되는 동안에도 자신의 작업을 처리하고 (Non Blocking), 다른 작업의 결과를 바로 처리하여 작업을 순차대로 수행 하는 (Sync) 방식입니다.</u>**

![image](https://github.com/user-attachments/assets/3e58bc74-fb1a-41c6-997f-07a686bbbe0c)




- 예시

  - 동기 + 논블로킹 코드를 표현하는데 적합한 대중적인 언어로 자바(Java)를 들 수있습니다.

    

``` java
// Runnable 인터페이스를 구현하는 클래스 정의
class MyTask implements Runnable {
    @Override
    public void run() {
        // 비동기로 실행할 작업
        System.out.println("Hello from a thread!");
    }
}

public class Main {
    public static void main(String[] args) {
        // Thread 객체 생성
        Thread thread = new Thread(new MyTask());

        // 스레드 실행
        thread.start();

        // Non-Blocking이므로 다른 작업 계속 가능
        System.out.println("Main thread is running...");

        // Sync를 위해 스레드의 작업 완료 여부 확인
        while (thread.isAlive()) {
            System.out.println("Waiting for the thread to finish...");
        }
        System.out.println("Thread finished!");
        
        System.out.println("Run the next tasks");
    }
}

```

- 스레드를 이용하여 작업을 병렬적으로 처리하도록 지시했지만, 
- 메인 코드의 while문을 수행함으로서 **<u>요청한 작업의 완료 여부를 계속 확인</u>**하고 결과적으로 결국 동기적으로 작업을 순서대로 수행됨을 볼 수 있습니다.

![image](https://github.com/user-attachments/assets/0663ca36-21f5-4ea9-8d7a-fd2a754665c4)






## **Async Blocking** 

- 다른 작업이 진행되는 동안 자신의 작업을 멈추고 기다리는 (Blocking), 다른 작업의 결과를 바로 처리하지 않아 순서대로 작업을 수행하지 않는 (Async) 방식입니다.

- 실무에서  다룰일이 거의 없다.

- 보통 Async-blocking은 개발자가 비동기 논블록킹으로 처리 하려다가 실수하는 경우에 발생하거나, 자기도 모르게 블로킹 작업을 실행하는 의도치 않은 경우에 사용된다. 그래서 이 방식을 안티 패턴(anti-pattern)이라고 치부하기도




![image](https://github.com/user-attachments/assets/a8d70b3e-0081-4d36-8c25-d8be84e1db92)






















