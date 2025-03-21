# AOP의 프록시 기반 방식과 바이트 코드 방식

<br>

### 1. 프록시 기반 AOP (spring aop)

- 프록시 기반 aop는 말 그대로 **대상 객체를 호출하기 전에 프록시 객체를 생성하고 프록시 객체가 대상 객체의 흐름을 가로채 필요한 부가기능을 실행한 뒤 대상 로직을 호출하는 방식**입니다.
- 스프링 AOP에서는 JDK Dynamic Proxy(인터페이스 기반) 및 CGLIB(클래스 기반)을 사용하여 프록시 객체를 생성합니다.



특징

- 런타임에 동적으로 프록시 객체를 생성
  - 스프링 애플리케이션이 구동되어질때 AOP 설정에 따라 필요한 **클래스에 대한 프록시 객체를 동적으로 생성**합니다.
- **스프링 AOP는 기본적으로 메서드 단위로 동작하기에 메서드 호출 시점에 부가기능을 삽입**합니다.
- 프록시 객체를 생성하는 방식이 두 가지가 존재한다.
  - 인터페이스 기반(JDK Dynamic Proxy) 또는 클래스 상속 기반(CGLIB)



장점

- 스프링 빈(bean) 설정만으로 쉽게 적용이 가능합니다.
- advice나 pointcut을 변경할때 비즈니스 로직을 수정할 필요가 없습니다.
  - AOP 설정(Advice, Pointcut)은  스프링의 설정 파일이나 @Aspect 클래스에서 관리하기 때문입니다.



단점

- `final` 클래스, private 메서드에 대한 제한이 있음
- 스프링 빈으로 관리되지 않는 객체에는 적용하기 어려움
- 호출 지점이 **메서드**로 한정되어 더 세밀한 AOP가 필요할 때는 한계가 있음

<br>

### 2. 바이트 코드 기반 AOP

- 바이트 코드 기반 AOP는 컴파일 타임이나 로드타임, 런타입에 직접 클래스의 바이트 코드를 조작하여 부가기능을 삽입하는 방식입니다.
- 대표적으로 aspectj 가 존재합니다.

특징

- 다양한 시점에서 위빙이 가능합니다.
  - 컴파일 타임, 로드 타임, 런타임 등등
- 클래스 내부 로직에 대해서 좀 더 깊이 있게 제어할 수 있습니다.
  - 프록시 AOP와 다르게 메서드 호출 뿐 만이 아니라 필드 접근, 생성자 호출 , static 초기화 블록등 다양한 지점에서 advice를 삽입할 수 있습니다.
- 프록시 AOP 방식보다 좀 더 다양한 포인트 컷 표현식을 제공하며 매우 세밀한 조건으로 AOP를 적용할 수 있습니다.

장점

- 메서드 외에도 **필드 접근, 생성자, static 블록, 예외 처리 지점** 등 다양한 Join Point에 적용 가능
- final 메서드, private 메서드 등의 제약이 거의 없음



단점

- 빌드/배포 과정이 다소 복잡해질 수 있음

- 학습 곡선이 프록시 AOP 보다 높습니다.

  

## 3. 어떤 방식을 선택해야 할까?

1. **간단한 메서드 레벨 로직 AOP**
   - 스프링 AOP 같은 **프록시 기반** 방식이 더 편리하고 빠르게 적용 가능합니다.
   - 주로 트랜잭션 처리, 로깅, 인증 등 Spring에서 자주 사용하는 횡단 관심사에 대해서는 스프링 프록시 AOP가 간편합니다.
2. **보다 정교하고 세밀한 AOP가 필요한 경우**
   - 생성자, 필드 접근, private 메서드, static 영역, 로우 레벨 로직에 대한 부가기능 적용이 필요하다면 **AspectJ**와 같은 바이트 코드 위빙 방식을 고려해야 합니다.
   - 대규모 시스템에서 성능이나 세밀한 모니터링, 또는 특정 라이브러리 호출을 가로채는 등의 복잡한 요구 사항이 있을 때 효과적입니다.
3. **프로젝트 환경・팀 역량**
   - 프록시 기반 AOP는 스프링 설정에 익숙한 개발자라면 적용하기 쉽습니다.
   - 바이트 코드 위빙은 별도 빌드 설정, 로더 설정 등에 대한 지식이 필요하므로 일정 학습 비용이 듭니다.
   - 팀 규모나 프로젝트 요구 사항, 배포 파이프라인 등을 종합적으로 고려해 결정하는 것이 좋습니다.
4. **성능 고려**
   - 일반적으로 프록시 기반 AOP는 런타임에 프록시를 생성하므로, 초기 구동 시점에 프록시를 만드는 비용이 발생하지만 큰 문제는 아닙니다.
   - 바이트 코드 위빙은 빌드 시점 혹은 로드 시점에 한 번 변환을 거친 뒤에는 호출 시 가로채기 오버헤드가 적습니다.
   - 성능 차이는 큰 틀에서 AOP 사용 방식보다는 **어드바이스 로직의 복잡도**에 더 영향을 받는 편입니다.
   - 극단적으로 수많은 클래스나 메서드에 AOP를 광범위하게 적용한다면, 바이트 코드 조작 방식이 더 일관된 성능을 낼 수도 있습니다.









https://hstory0208.tistory.com/entry/Spring-%EC%8A%A4%ED%94%84%EB%A7%81-AOPAspect-Oriented-Programming%EB%9E%80-Aspect



https://hstory0208.tistory.com/243

















