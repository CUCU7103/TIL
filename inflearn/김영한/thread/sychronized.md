# sychronized 



![image-20250104235700605](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20250104235700605.png)

- 모든 객체(인스턴스)는 내부에 자신만의 락(lock)을 가지고 있습니다.
  - 모니터 락이라고도 부릅니다.
  - 객체 내부에 있고 우리가 확인하기는 어렵다.
- 스레드가 `synchronized` 키워드가 있는 메서드에 진입하려면 반드시 해당 인스턴스의 락이 있어야 한다!
  - 여기서는 `BankAccount(x001)` 인스턴스의 `synchronized withdraw()` 메서드를 호출하므로 이
    인스턴스의 락이 필요하다.

- 스레드 `t1` , `t2` 는 `withdraw()` 를 실행하기 직전이다.

  

![image-20250105000359908](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20250105000359908.png)

- t1이 먼저 실행된다고 가정한다면
- 스레드 t1이 먼저 sychronized 키워드가 있는 withdraw() 메서드를 호출한다.
- sychronized 메서드를 호출하려면 먼저 해당 인스턴스의 락이 필요합니다.
- 락이 있으므로 스레드 t1은 BankAccount 인스터스에 있는 락을 획득한다.

![image-20250105000529863](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20250105000529863.png)

- 스레드 t1은 해당 인스턴스의 락을 획득했기 때문에 withdraw() 메서드에 진입할 수 있다
- t2는 메서드 호출을 시도하지만 해당 인스턴스의 락이 없기에 락을 획득할 때까지 blocked 상태로 대기합니다.
  - t2의 상태는 Runnable -> blocked 상태로 변하고 , 락을 획득할때까지 무한정 대기한다.

![image-20250105000737453](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20250105000737453.png)

![image-20250105000745621](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20250105000745621.png)

![image-20250105000751257](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20250105000751257.png)

![image-20250105000801276](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20250105000801276.png)

![image-20250105000807749](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20250105000807749.png)

![image-20250105004539447](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20250105004539447.png)
