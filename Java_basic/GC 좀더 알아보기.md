## GC ROOT란?
- GC가 동작하는 방식은 Mark & sweep 방식으로 GC는 Unreachable한 객체를 제거하고 메모리를 회수합니다.
- 이때 Mark & Sweep 방식은
  - Mark 과정에서 **GC root로 부터 객체에 접근**, 즉 참조의 여부로 객체 reachable, unreachable을 판단합니다.
  - sweep 과정에서 Unreachable한 객체를 제거합니다.
  
- GC root는 gc를 실행할때 Mark & sweep 방식 중 mark 단계에서 ROOT 부터 그래프 순회를 시작하고 이를 통해서 연결된 객체들을 찾아내어 마킹합니다.
### 즉 GC ROOT는 heap영역의 객체를 참조하고 있는 영역이라고 볼 수있습니다.
- jvm에서 heap영역의 객체를 참조하는건 stack 영역 method 영역, native method stack입니다.

![image](https://github.com/user-attachments/assets/aa7214d6-3299-4857-813c-be28883b2115)


## GC의 종류 

![image](https://github.com/user-attachments/assets/69302c0a-2a30-4c30-bc09-97ad311b5ee4)

모든 gc에서는 stop the world가 발생합니다.
stop the world : 
gc가 실행되어지면 gc 실행중인 스레드를 제외하고 모든 스레드가 멈춥니다.

1.Minor GC
  - Heap 메모리의 young generation에서 발생하는 GC를 말합니다.
  - 메모리의 크기가 old generation 보다 상대적으로 작아 빠른 작업 속도를 가집니다.
  - stop the world 현상로 인한 문제가 거의 발생하지 않습니다.
  
2.Majar GC 
  - Heap 메모리의 old generation이 가득 차면 동작합니다.
  - Minor GC 진행중 객체의 age-bit가 임계값에 도달하면 old generation으로 넘깁니다.
  - Old Generation은 Young Generation에 비해 상대적으로 큰 공간을 가지고 있어, 이 공간에서 메모리 상의 객체 제거에 많은 시간이 걸리게 됩니다.
  - 이때 객체를 제거하는데 시간이 많이 걸리게 되어지고 stop the world 현상이 길게 유지되어집니다. 

3. Full GC
   - 전체 Heap 영역에서 동작하는 GC입니다.
   - GC 중 가장 오랫동안 stop the world 현상이 유지되어집니다.
   - 힙 전체를 대상으로 하므로 객체의 마킹, 스위핑, 힙 압축(compaction) 등을 모두 포함합니다.
      
## young generation에서 Minor GC가 동작하는 방식
   1. Eden 영역에 객체들이 가득차서 GC가 발생합니다
   2. 이때 s0에 mark된 객체들이 복사되어지고 해당 객체들에 age가 1씩 부여됩니다. 그리고 Eden의 영역을 비웁니다.
   3. Eden 영역에 가득차서, GC가 다시 한번 발생합니다
   4. 그러면 기존 Eden에서 mark된 객체들과 s0에 있는 객체들은 s1 로 복사되어진뒤 각 eden과 s0은 비워집니다. 마찬가지로 age가 1씩 증가합니다.
   5. 이렇게 반복되어지다 객체의 age가 15가 되면 old generation으로 넘어갑니다.

## survior가 두 개인 이유 

  두 개의 survivor 영역을 사용함으로써 항상 하나의 비어있는 영역을 확보할 수 있습니다
  즉 빠른 메모리 복사, 그리고 메모리 청소과정이 단순해집니다.
  이에 따른 이점으로는
  1. 메모리 단편화를 방지할 수있습니다.
     - 빈 공간에 메모리를 바로 복사하고, 나머지 공간의 메모리를 청소하므로써 메모리 단편화를 방지할 수 있습니다.
  2. 객체의 age 관리
     - 새로운 survivor 공간으로 복사되어질때 마다 age를 증가시키고
       일정한 age값이 되었을경우 old generation으로 Promotion 시켜 메모리를 효율적으로 관리합니다.
  3. GC의 성능향상
     - 두 개의 survior 영역을 사용하므로써 항상 하나의 비어잇는 영역을 확보할 수 있고 이에 따라서 빠른 메모리 복사와 청소작업이 가능해집니다.
     - 이로 인해 stop the world로 발생되어지는 응답 지연시간을 최소화 할 수 있습니다.

## Stop the world가 느린 이유
stop the world가 발생하면 gc 작업이 진행되는 스레드를 제외하고 모든 스레드가 멈춥니다. <br>
즉 gc의 스레드 작업이 마무리 되어야지 어플리케이션이 다시 정상적으로 동작합니다.<br>
이때 Heap 영역이 크기가 크면 GC가 Mark & Sweep 과정을 수행하는데 시간이 많이 걸리고 이로 인해서 stop the world 시간이 느려집니다.

    

