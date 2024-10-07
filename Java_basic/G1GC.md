# G1GC

 - Garbage First 라는 이름으로 지어진 GC 입니다.
 - 큰 메모리를 갖는 다중 프로세서 시스템을 대상으로 합니다.
 - 자바 9에 들어오면서 default gc가 되었습니다.

### 장점
- **예측 가능한 성능**:  **목표 지연 시간(Target Pause Time)** 을 설정할 수 있기 때문에, 원하는 성능 목표를 유지하는 데 도움이 된다.
- **대용량 힙에 대한 지원**: G1 GC는 대용량 힙을 가진 애플리케이션에 이상적이다. 다른 GC 알고리즘에 비해 더욱 효율적으로 메모리를 관리할 수 있다.
- **별도의 STW 없이도 여유 메모리 공간을 압축하는 기능을 제공** :

###  힙 메모리의 구성
  - Region : 힙을 동일한 크기의 작은 Region으로 나누어 관리합니다. 이렇게 하면 G1GC는 필요한 작업을 더 세밀하게 관리할 수 있고, 특정 영역에 대한 집중적인 작업이 가능해집니다.  
  - Eden, Survivor, Old Region: 각 Region은 역할에 따라 세 가지 유형으로 나뉩니다
    - Eden Region: 새로운 객체가 할당되는 공간으로, 새로 생성된 객체들이 먼저 여기에 저장됩니다.
    - Survivor Region: Eden에서 살아남은 객체들이 이동하는 공간으로, 여러 Young GC를 거친 뒤에도 여전히 참조되고 있는 객체들이 이곳으로 옮겨집니다.
    - Old Region: 오래된 객체가 할당되는 공간으로, 일정 수의 Young GC를 거친 뒤에도 살아남은 객체들이 여기로 이동합니다.
   
  - Humongous Region: 매우 큰 객체(Region의 50%보다 큰 객체)는 Humongous Region에 저장됩니다. 이러한 객체는 일반 Old Region과는 다르게 처리됩니다.


### G1GC의 동작과정
- G1GC의 주요 동작 과정은 Minor GC, Concurrent Marking, 그리고 Mixed GC 단계로 나뉩니다

- Minor GC
- Eden 영역의 수집: G1GC의 Young GC 단계는 Eden Region이 꽉 찼을 때 수행됩니다. Young GC는 Eden Region의 객체들을 가비지 컬렉션 대상으로 수집합니다.
- 객체 이동: Young GC 동안 Eden에서 살아남은 객체는 Survivor Region으로 이동하게 되고, 참조되지 않는 객체는 가비지로 수집됩니다. 이로 인해 Eden 영역은 메모리가 확보됩니다.
- Stop-the-World 이벤트: Young GC는 짧은 Stop-the-World (STW) 이벤트로 진행됩니다. 이 단계는 주로 빠르게 끝나며, 지연 시간이 짧은 것이 특징입니다.

- Concurrent Marking
- G1GC의 핵심 작업 중 하나로, **힙의 상태를 분석하고 Old Region에서 어떤 객체가 여전히 사용되고 있는지 확인하는 과정**을 거칩니다.
  - 이 단계는 여러 하위 단계로 나뉩니다.
    - Initial Mark: Initial Mark는 Young GC 중에 수행되며, GC Root에 연결된 객체를 마크하는 작업입니다. 
    - Root Region Scan: Root Region Scan 단계에서는 Old Region에 있는 객체들이 GC Root에 의해 참조되고 있는지 여부를 탐색합니다. 이를 통해 Old Region의 일부 객체가 수집되지 않도록 합니다.
    - Concurrent Mark: Concurrent Mark는 애플리케이션 스레드와 병렬로 수행됩니다. 이 과정에서 **G1GC는 전체 힙을 스캔하여 살아 있는 객체들을 마크**합니다. **애플리케이션의 동작을 방해하지 않도록 동시에 진행됩니다.**
    - Remark: Remark 단계는 마지막으로 한 번 더 Stop-the-World 이벤트가 발생하여, 마크되지 않았거나 변경된 객체 참조 정보를 확인하고 갱신합니다.
    - Cleanup: Cleanup 단계에서는 수집할 Region을 결정합니다. 일부 작업은 STW로 수행되고, 나머지 작업은 애플리케이션과 병렬로 수행됩니다. 이 단계에서는 가비지가 많이 쌓인 Region을 회수하여 메모리를 확보합니다.


- Mixed GC
- Young + Old 영역 수집: Mixed GC는 Young GC와 비슷하게 Young Region과 일부 Old Region을 함께 수집합니다. 이를 통해 Old Region에 있는 오래된 객체들도 함께 정리할 수 있습니다.
- 가비지 비율 높은 Region 우선 수집: G1GC는 힙 내에서 가비지 비율이 높은 Region을 우선적으로 수집합니다. 이러한 방식으로 Mixed GC는 Young GC만으로는 충분히 메모리를 확보하지 못할 때, Old Region도 수집하여 힙의 메모리 상태를 개선합니다.
- Collection Set (CSet): G1GC는 **Collection Set(CSet)** 이라는 개념을 통해 수집할 Region을 정의하고, Mixed GC 동안 이 CSet에 포함된 Region을 대상으로 가비지 컬렉션을 수행합니다.








