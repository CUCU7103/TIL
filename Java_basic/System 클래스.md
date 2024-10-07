# System 클래스

- System 클래스의 가장 큰 특징은 생성자가 없다는 것입니다.
- System 클래스는 3개의 스태틱 변수가 선언되어 있습니다.
- 시스템의 정보를 출력하기 위한 클래스 입니다.
  - 즉 파일 입출력 관련 메서드를 찾으려면 PrintStream 클래스에서 찾아야 합니다.

![image](https://github.com/user-attachments/assets/6674f8e6-b3ae-4af7-bfa2-623112e52bc6)


### 시스템의 속성 값 관리 메서드
![image](https://github.com/user-attachments/assets/b3fc8c56-90d4-4150-80c9-64de225e083e)

getProperty 메서드를 사용하여 시스템에 설치되있는 자바 버전을 확인하였습니다.
```
public class javaLangSystem {

    public static void main(String[] args) {
        System.out.println("java version = " + System.getProperty("java.version"));
    }
}

```

### 시스템의 환경 값을 조회하기

![image](https://github.com/user-attachments/assets/8bde1af6-875c-4a91-9357-3724782aaa24)

- .env는 환경값이며 이는 OS나 장비에 관련된 것들이기에 수정이 불가능하다.
- 


```

 System.out.println("JAVA_HOME="+ System.getenv("JAVA_HOME"));
 // JAVA_HOME=C:\Program Files\Java\jdk-17
```


### GC 수행 / JVM 종료

![image](https://github.com/user-attachments/assets/bca6a54b-4fc4-47bf-b706-20460dfb3215)

![image](https://github.com/user-attachments/assets/732ee5c1-17ee-4300-bb47-07e660a9fa73)

- Java에서는 GC를 직접하지 않고 JVM에서 처리해준다 직접 건드릴 필요가 없는 부분이다.
- JVM 종료는 말 그대로 애플리케이션이 강제종료된다 정말 쓸 필요가 없다.
  - 매개변수에 0이 넘어가면 정상종료이고 그 이외의 숫자는 비정상 종료이다.



### 현재시간 조회

![image](https://github.com/user-attachments/assets/9f4f5b3e-aebb-4462-aa92-dd1899a5a80a)

- currentTimeMills()는 현재 시간을 확인하기 위해서 사용되어집니다.
- nanoTime()은 주로 시간의 차이를 측정하기 위해서 사용되어집니다.
  

## System.out,err

- System 클래스에 선언되어 있는 out과 err 변수는 PrintStream이라는 동일한 클래스의 객체이다.
- out은 정상적인 출력을 err는 에러가 발생했을 때의 출력을 할 때 쓰인다.

```
public class ImmutableClassTest {
    public static void main(String[] args) {
        printNull();
    }

    public static void printNull() {
        Object obj = null;
        System.out.println(obj);
        System.out.println(obj + " is object's value");
    }
    
}

//
null
null is object's value
```

- 첫번째 출력문이 "null"로 출력되는 이유는, 객체.toString()이 아닌, String.valueOf(객체)의 값이 출력되기 때문입니다.```System.out.println(String.valueOf(null));```
과 같은 것입니다. null에다 toString을 호출하면 NullPointException이 발생한다.
- 참고로 객체의 값을 출력할 때, 객체.toString()으로 호출하는 것보다, String.valueOf(객체)를 호출해서 출력하는 것이 더 안전합니다.

- 두 번째도 제대로 출력이 되는 이유는 obj를 StringBuilder 클래스 append() 메소드에 파라미터로 넘겨 출력을 했기 때문입니다.
```System.out.println(new StringBuilder().append(obj).append(" is object's value")); ```











