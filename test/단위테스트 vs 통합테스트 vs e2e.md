# 단위테스트 vs 통합테스트 vs e2e테스트 (구글 테스트 피라미드)



![image](https://github.com/user-attachments/assets/5301261b-d139-4c3c-bc7c-4e90cc4088ca)






## 단위 테스트란?

- 응용프로그램에서 테스트 가능한 가장 작은 소프트웨어를 실행하여 예상대로 동작하는지 확인하는 테스트 입니다.

- 애플리케이션 안에 있는 **개별적인 코드 단위**가 의도한 대로 작동하는지 확인하는 행위입니다

  - #### **즉 단위 테스트는 클래스나 메서드 단위에서 해당 코드가 우리가 의도한 대로 작동하는지 확인하는 행위입니다.**

- 또한  단위테스트는 주로 테스트하고자 하는 기능이나 메서드만을 초점을 맞추고, 이를 위해 필요한 의존성은 Mockito와 같은 도구를 사용해 모의(Mock) 객체로 대체합니다.

![image](https://github.com/user-attachments/assets/416b2f08-1905-40b5-b80e-b276bb695ff3)


- 이러한 애플리케이션의 기능들을 하나씩 개별적으로 테스트 하는 것을 의미합니다.

## 단위 테스트를 왜 사용하는가

- 원하는 부분만 테스트를 함으로 결과를 빠르게 볼 수 있습니다.
- 미리 작성한 단위 테스트를 기반으로 프로덕션 코드의 리펙터링을 안전하게 할 수 있습니다.
- 단위 테스트가 실패하는 지점에서 문제점을 빠르게 찾을 수있다.

정리하자면 코드의 일부분을 빠르게 검증할 수 있고, 코드 리펙터링이 안정적이고, 코드의 문제점을 빠르게 찾을 수있습니다.



예시 클래스

``` java
public class Car {
    
    private int position;
    
    public Car(){
        this.position = 0;
    }
    
    
    // 명령한 수 만큼 move() 메서드를 실행시키는 기능
    public void moveAsOrdered(final int orderCount){
        this.position += orderCount;
    }
    
    
    public int getPosition(){
        return position;
    }
    
    
}

==================================================================
 // 단위 테스트
 @Test
 @DisplayName("자동차는 명령을 받은 만큼 이동거리가 증가합니다.")
 void car_moves_as_ordered(){
    int orderCount = 10;
    Car car = new Car();
    car.moveAsOrdered(orderCount);
    int distanceResult = car.getPosition();
    assertThat(distanceResult).isEqualsTo(orderCount);
}
=================================================================
 // given - when - then
 @Test
 @DisplayName("자동차는 명령을 받은 만큼 이동거리가 증가합니다.")
 void car_moves_as_ordered(){
    // given
    int orderCount = 10;
    Car car = new Car();
    // when
    car.moveAsOrdered(orderCount);
    int distanceResult = car.getPosition();
    // then
    assertThat(distanceResult).isEqualsTo(orderCount);
}
 


```

## 테스트 코드의 작성 패턴

1. given - when -then
   - given 
     - 테스트를 위해 준비하는 과정
     - 테스트에 사용되는 값을 정의합니다
   - when
     - 테스트하고자 하는 기능을 실행시킵니다.
   - then
     - 기능을 실행한 후 결과를 검증합니다. 

- 테스트를 더 쉽게 읽을 수 있고 유지보수 하기 좋다
- 필수가 아닌 선택



### @ParameterizedTest

- 여러 값을 한개의 테스트 메서드에서 검증할 수있다.

``` java
@ParameterizedTest
@ValueSource(ints = {-1,0,11,12})
@DisplayName("자동차를 이동시킬 거리가 1 이상 10 이하의 수가 아니면 예외가 발생한다.")
void  throws_exception_when_order_count_invalid(int givenOrderCount){
    // given
    String expectedErrorMessage = "이동시킬 거리는 1 이상 10 이하의 수만 가능하다."
    Car car = new Car();
    
    // when & then 
    assertThatThrownBy(() -> car.moveAsOrdered(givenOrderCount))
        .isInsteadOf(IllegalArgumentException.class)
        .hasMessageContaining(expectErrorMessage);
}
```



### 단위테스트의 원칙 FIRST

- Fast
  - 단위 테스트는 빨라야 한다.
- Independent
  - 테스트는 독립적으로 동작해야 한다.
  - 이전 테스트에 영향을 받으면 실패원인을 찾기 어렵습니다
- Repeatable 
  - 어떤 상황에서든 예상한 대로 테스트 결과가 나와야 한다.
- Self-Validating
  - 출력 혹은 로그가 아닌 테스트 자체적으로 결과가 나와야 한다.
- Timely
  - 적시에 테스트를 철저하게 작성해야한다.



## Q&A  (Feat.gpt)

- 단위 테스트는 mock 객체 없이도 테스트가 가능합니다. 하지만 서비스 레이어, 리포지토리 레이어에서 단위 테스트를 진행하려면 결국 mock 객체가 없으면 테스트가 어렵지 않나요???

---

단위 테스트에서 Mock 객체를 사용하는 것은 **필수적**은 아니지만, 일반적으로 **서비스나 리포지토리** 레이어를 테스트할 때는 종속성을 격리하기 위해 Mock 객체를 사용하는 것이 유리합니다.

특히, 외부 시스템(DB, API 등)과의 의존성을 줄이고 테스트를 독립적으로 유지하려면 Mock을 사용하는 것이 일반적입니다.

아래는 **Spring Boot** 기반 프로젝트에서 각 레이어별 간단한 단위 테스트 예시를 제공합니다.

------

### 1. **컨트롤러 레이어 (Controller Layer) 테스트**

```java
@RestController
public class UserController {
    private final UserService userService;

    public UserController(UserService userService) {
        this.userService = userService;
    }

    @GetMapping("/users/{id}")
    public ResponseEntity<User> getUser(@PathVariable Long id) {
        return ResponseEntity.ok(userService.getUserById(id));
    }
}
```

**테스트 코드 (컨트롤러 레이어 단위 테스트):**

```java
@WebMvcTest(UserController.class)
class UserControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private UserService userService;

    @Test
    void getUser_shouldReturnUser() throws Exception {
        User user = new User(1L, "John Doe");
        given(userService.getUserById(1L)).willReturn(user);

        mockMvc.perform(get("/users/1"))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.id").value(1))
                .andExpect(jsonPath("$.name").value("John Doe"));
    }
}
```

------

### 2. **서비스 레이어 (Service Layer) 테스트**

```java
@Service
public class UserService {
    private final UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public User getUserById(Long id) {
        return userRepository.findById(id).orElseThrow(() -> new RuntimeException("User not found"));
    }
}
```

**테스트 코드 (서비스 레이어 단위 테스트):**

```java
@ExtendWith(MockitoExtension.class)
class UserServiceTest {

    @Mock
    private UserRepository userRepository;

    @InjectMocks
    private UserService userService;

    @Test
    void getUserById_shouldReturnUser() {
        User user = new User(1L, "John Doe");
        given(userRepository.findById(1L)).willReturn(Optional.of(user));

        User result = userService.getUserById(1L);

        assertEquals(1L, result.getId());
        assertEquals("John Doe", result.getName());
        verify(userRepository, times(1)).findById(1L);
    }
}
```

------

### 3. **리포지토리 레이어 (Repository Layer) 테스트**

```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    // Constructors, Getters, Setters
}
```

**테스트 코드 (리포지토리 단위 테스트):**

```java
@DataJpaTest
class UserRepositoryTest {

    @Autowired
    private TestEntityManager entityManager;

    @Autowired
    private UserRepository userRepository;

    @Test
    void findById_shouldReturnUser() {
        User user = new User();
        user.setName("John Doe");
        entityManager.persist(user);
        entityManager.flush();

        Optional<User> foundUser = userRepository.findById(user.getId());

        assertTrue(foundUser.isPresent());
        assertEquals("John Doe", foundUser.get().getName());
    }
}
```

``` java
// EntityManager 없이 리포지토리 테스트 
@DataJpaTest
class UserRepositoryTest {

    @Autowired
    private UserRepository userRepository;

    @Test
    void findById_shouldReturnUser() {
        // 데이터 저장
        User user = new User();
        user.setName("John Doe");
        userRepository.save(user); // 리포지토리 사용

        // 데이터 조회
        Optional<User> foundUser = userRepository.findById(user.getId());

        assertTrue(foundUser.isPresent());
        assertEquals("John Doe", foundUser.get().getName());
    }
}

```

### 주요 사항

1. **Mock 사용 여부**:
   - **서비스 레이어**: 
     - 보통 `Mock` 객체를 사용하여 DB나 외부 API 호출을 대체합니다.
   - **컨트롤러 레이어**: 
     - `@MockBean`으로 의존성을 목킹.
   - **리포지토리 레이어**: 
     - `@DataJpaTest`를 사용하여 실제 데이터베이스와 연결된 환경에서 테스트.
       
2. **테스트 어노테이션**:
   - `@WebMvcTest`: 컨트롤러 레이어 테스트
   - `@ExtendWith(MockitoExtension.class)`: Mock을 사용한 서비스 테스트
   - `@DataJpaTest`: JPA 리포지토리 테스트




## 통합테스트란?

- 통합 테스트는 전체 **스프링 부트 애플리케이션 컨텍스트를 로드**하여 실제 환경과 유사한 조건에서 컨트롤러의 동작을 테스트하는 것입니다. 

  - 스프링 부트 애플리케이션 컨텍스트를 로드한다?
    - 실제 구동되어지는 애플리케이션의 설정, 모든 bean을 로드한다는 의미입니다.
  - 목적은 모듈 간의 인터페이스, 데이터 흐름, 외부 시스템과의 연동 등을 검증하는 것이다. MockMvc를 사용하여 HTTP 요청을 모의로 보내고, 응답을 검증합니다.

- **외부 의존성을 수반하는 테스트 입니다.**

  - 실제 데이터베이스, 파일 시스템, 네트워크 서비스 등과의 상호작용을 포함하여, 더 현실적인 시나리오에서 시스템의 동작을 검증하는 테스트 입니다.

- #### 즉 **통합테스트는 외부 의존성을 사용하여 실제 환경과 유사한 환경을 구축하고, 애플리케이션의 모듈 단위로 HTTP 요청을 모의로 보내 응답을 검증하는 테스트입니다.**  

  

### 통합테스트의 특징

- **전체 흐름 검증**: 
  - 사용자 요청이 컨트롤러를 통해 서비스 계층으로 전달되고, 서비스 계층에서 비즈니스 로직을 처리한 후 레포지토리를 통해 데이터베이스와 상호 작용하는 전체 흐름을 테스트합니다

- **실제 환경 사용**: 
  - 데이터베이스, 웹 서버 등 실제 또는 유사한 환경에서 테스트합니다.
- **기능중심 테스트**: 
  - 개별 메서드나 클래스가 아닌 웹 서버등을 사용하여

### 통합 테스트의 예시

- 웹 애플리케이션에서 사용자가 로그인을 시도할 때, 인증 시스템과 데이터베이스가 올바르게 통신하는지 검증하는 테스트 , 게시글에 대한 조회 등 하나의 **전체적인 기능을 테스트** 할때 사용되어집니다.
- 애플리케이션의 **모듈**에 대한 테스트



### 적용하는 상황

- **기능 통합**:
  - 여러 모듈이나 서비스가 통합될 때, 통합 테스트는 이들 간의 상호작용이 제대로 이루어지는지 확인한다.
- **시스템 확장**:
  - 애플리케이션이나 시스템에 새로운 기능이나 서비스를 추가할 때, 통합 테스트는 새로운 요소가 전체 시스템과 잘 호환되는지 검증해 준다.
- **배포 전 검증**: 
  - 실제 사용 환경으로의 배포 전, 통합 테스트는 애플리케이션이 실제 운영 환경에서 예상대로 작동할 것인지 확인하는 데 중요하다.

### 통합테스트와 단위테스트의 비교 

| 항목            | 단위 테스트                   | 통합 테스트                            |
| --------------- | ----------------------------- | -------------------------------------- |
| **대상 범위**   | 개별 클래스 또는 메서드       | 여러 모듈이 연계된 기능 전체           |
| **의존성 처리** | 모킹(Mock)을 통해 의존성 격리 | 실제 의존성 사용 또는 최소한의 모킹    |
| **테스트 속도** | 빠름                          | 상대적으로 느림                        |
| **복잡도**      | 낮음                          | 높음                                   |
| **주요 목적**   | 개별 로직의 정확성 검증       | 시스템의 통합된 동작 및 상호 작용 검증 |
| **실행 환경**   | 단순한 테스트 환경            | 실제 또는 실제에 가까운 환경           |



# e2e test (end to end test)

- 애플리케이션의 시작부터 끝까지의 전체적인 흐름을 실제 사용자 관점에서 테스트하여, 시스템이 예상대로 동작하는지를 검증하는 테스트 방법입니다.

- ### **E2E 테스트의 목적**

  - **사용자 경험 검증**
    - 시스템이 사용자에게 기대하는 기능과 성능을 제공하는지 확인합니다.
  - **시스템 통합 확인**
    - 모든 구성 요소 간의 통합이 원활하게 이루어지는지 검증합니다.
  - **품질 향상**
    - 전체적인 시스템의 안정성과 신뢰성을 높입니다.

  





















