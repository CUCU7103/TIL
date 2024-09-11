## JPA Auditing

- jpa에서 auditing 이란 '감시하다' 라는 뜻으로, 각 데이터 마다 누가, 언제 데이터를 생성했고 변경했는지 감시한다는 의미로 사용되어진다.
- 아래와 같은 필드에 대표적으로 많이 사용되어진다.
  -- 생성 주체
  -- 생성 일자
  -- 변경 주체
  -- 변경 일자
  

### 사용 예시

![image](https://github.com/user-attachments/assets/d8fab762-ecb3-40ef-b8a2-63b8a83172f1)

```java

@EnableJpaAuditing
@Configuration
public class JpaAuditingConfig {
    // Spring JPA Auditing 사용을 위한 클래스 생성
    // @SpringBootApplication 가 붙은 곳에도 사용이 가능하나 테스트 코드 작성시 오류가 발생할 수 있다고 하여 별도의 클래스로 생성함
    // @Configuration 이 붙어있는 클래스에 @EnableJpaAuditing를 적용하면된다. 역할을 명확이 하기 위해 별도의 클래스로 만들었다.
}



```

- 적용할 엔티티 클래스

```java
@Entity
@Table(name="tbl_member")
@Getter
@ToString
@AllArgsConstructor
@NoArgsConstructor
@Builder
@EntityListeners(value = {AuditingEntityListener.class}) // 해당 어노테이션을 적용해야  auditing 기능을 사용할 수 있다.
public class MemberEntity { 

    @Id
    private String mid;

    private String mpw;

    private String mname;

    private String email;

    @CreatedDate // 생성일자를 자동으로 입력
    private LocalDateTime joinDate;

    @LastModifiedDate // 변경일자를 자동으로 입력
    private LocalDateTime modifiedDate;

    // 일반 회원, 관리자.
    private String role;

    public void changePassword(String password){
        this.mpw = password;
    }

    public void changeName(String name){
        this.mname = name;
    }

    public void changeEmail(String email){
        this.email = email;
    }

    public void changeRole(String role){
        this.role = role;
    }

}


```

- 보통 생성일자나 변경일자 같은 경우에는 각 엔티티에 중복되어져서 들어가는 경우가 많아 ```BaseEntity```와 같은 클래스를 별도로 만들어서 해당 엔티티를 상속받아 사용하게 하는 방법도 있다.

```java
 @Data
 @MappedSuperClass // 엔티티 클래스가 상속받을 경우 자식 클래스에게 매핑 정보를 전달함.
 @EntityListeners(value = {AuditingEntityListener.class}) 
 public class BaseEntity {


    @CreatedDate // 생성일자를 자동으로 입력
    private LocalDateTime joinDate;

    @LastModifiedDate // 변경일자를 자동으로 입력
    private LocalDateTime modifiedDate;

}
```

- 그러면 기존의 ```MemberEntity```는  아래와 같이 변경되어진다.

```java

@Entity
@Table(name="tbl_member")
@Getter
@AllArgsConstructor
@NoArgsConstructor
@Builder
public class MemberEntity extends BaseEntity{ 

    @Id
    private String mid;

    private String mpw;

    private String mname;

    private String email;


    // 일반 회원, 관리자.
    private String role;

    public void changePassword(String password){
        this.mpw = password;
    }

    public void changeName(String name){
        this.mname = name;
    }

    public void changeEmail(String email){
        this.email = email;
    }

    public void changeRole(String role){
        this.role = role;
    }

}


```

