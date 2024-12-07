# JPA의 프록시란?

> [!TIP]
>
> 프록시(proxy)란?
>
> - `JPA`에서 **프록시는 실제 엔티티 객체 대신 데이터베이스 조회를 지연할 수 있는 가짜 객체**를 의미합니다. 
>
> - 가짜 객체라고 해서 실제 엔티티의 동작을 수행하지 못하는 것은 아닙니다. 
>
>   - 실제 엔티티 클래스를 상속받아 만들어지므로 실제 클래스와 겉모양이 같으므로 사용하는 입장에서는 진짜 객체인지 가짜 객체인지 구분하지 않고 사용하면 됩니다.
>
>   ![image-20241208014319107](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20241208014319107.png)
>
>   
>
> - 겉모양은 같아 사용하는 입장에서는 동일하게 사용하면 되지만 내부적으로는 다른 동작을 수행합니다.
>
> - **프록시 객체는 실제 객체에 대한 참조를 가지고 있기에 프록시 객체의 메서드를 호출하면 프록시 객체는 참조를 통해 메서드 호출을 위임하고 실제 객체의 메서드를 호출하게 됩니다.**



```java
public class JpaMain {

    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
        EntityManager em = emf.createEntityManager();
        EntityTransaction tx = em.getTransaction();

        tx.begin();

        try {
            Member member = new Member();
            member.setName("woochang");
            em.persist(member);

            em.flush();
            em.clear();
            
            // 실제 엔티티를 가져오는법
            // Member findMember = em.find(Member.class, member.getId());
            // System.out.println(findMember.getClass().getName());
            
			// getReference()를 통해 프록시 객체를 가져온다.
            Member findMember = em.getReference(Member.class, member.getId());
            System.out.println(findMember.getClass().getName());

            tx.commit();
        } catch (Exception e) {
            tx.rollback();
        } finally {
            em.close();
        }

        emf.close();
    }

}

// 하지만 아직 실제 엔티티 객체의 참조를 가지고 있지는 않습니다.
```



### 프록시 객체의 초기화 

- 프록시 객체는 **메서드 호출 등과 같이 실제 사용될 때 데이터베이스를 조회해서 실제 엔티티 객체를 생성 후 해당 객체를 참조**로 가지게 됩니다. 
- 이를 프록시 객체의 초기화라고 합니다.



![image-20241207193437813](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20241207193437813.png)

### 진행과정

1. getReference 호출을 통해 프록시 객체를 생성합니다.
   - `getReference()` 호출 시 엔티티가 이미 **영속성 컨텍스트에 관리**되고 있다면, JPA 구현체는 새로운 프록시 객체를 만들지 않고 기존 엔티티 인스턴스를 반환합니다.
   - 이는 동일한 영속성 컨텍스트 내에서 동일한 엔티티 식별자에 대한 단일 엔티티 인스턴스를 보장하는 일관성을 유지하기 위함입니다.

2. 실제 사용을 위해 메서드를 호출합니다.
3. 메서드가 호출되면 영속성 컨텍스트에 초기화를 요청합니다. 
   - 실제 엔티티 객체의 참조를 가지기 위해 실제 엔티티 객체의 생성이 필요합니다.
4. 영속성 컨텍스트는 데이터베이스를 조회해서 실제 엔티티 객체의 정보를 얻습니다
5. 조회된 정보를 바탕으로 실제 Entity 객체를 생성합니다.
6. 참조를 통해 실제 엔티티 객체의 메서드를 호출합니다.
7. 메서드 호출의 결과를 반환합니다.

<br>

### 프록시의 특징

- **프록시 객체는 처음 사용할 때 한 번만 초기화**
- 프록시 객체를 초기화 할 때, 프록시 객체가 실제 엔티티로 바뀌는 것은 아님, **초기화되면 프록시 객체를 통해서 실제 엔티티에 접근 가능**
- 프록시 객체는 원본 엔티티를 상속받은 객체이므로 타입 체크 시 주의 해야함
- 영속성 컨텍스트에 이미 존재한다면 데이터베이스 조회가 필요 없고 프록시 객체 사용 필요성이 없으므로 프록시 객체가 아닌 실제 엔티티 객체를 반환한다.
- 영속성 컨텍스트에 찾는 엔티티가 이미 있으면 entityManager.getReference()를 호출해도 실제 엔티티를 반환한다.
- **영속성 컨텍스트의 도움을 받을 수 없는 준영속 상태일 때, 프록시를 초기화하면 문제 발생 -> 하이버네이트는 LazyInitializationException 예외를 터트림**
  - 프록시 객체가 아직 초기화되지 않은 상태에서 영속성 컨텍스트 범위를 벗어나 접근하면 DB 조회를 할 수 없으므로 `LazyInitializationException`이 발생합니다.
  - 이는 JPA 표준 예외는 아니지만, 하이버네이트 등 구현체에서 일반적으로 발생하는 예외입니다. 
  - 이 문제를 피하려면 트랜잭션 범위 내에서 필요한 엔티티에 접근하거나, 미리 초기화 혹은 페치 전략을 적절히 설정해야 합니다.

<br>

**즉시 로딩과 지연 로딩**

- 프록시는 주로 `LAZY` 설정에서 사용됩니다. 
  - `EAGER`로 설정하면 애초에 **JPA가 즉시 엔티티를 로딩하므로 프록시 대신 실제 엔티티 객체를 반환**합니다.
  - 프록시는 지연 로딩을 위한 핵심 메커니즘입니다.

- 즉시로딩

  - **즉시 로딩**은 엔티티를 조회할 때 관련된 모든 연관 엔티티를 함께 즉시 로드하는 전략입니다.

  - 즉, 메인 엔티티를 조회하는 동시에 연관된 엔티티들도 함께 조회되어 한 번의 쿼리 또는 여러 쿼리로 데이터를 가져옵니다.

    

- 지연로딩

  - **지연 로딩**은 실제로 연관된 데이터가 필요할 때까지 해당 데이터를 로드하지 않는 전략입니다. 
  - 즉, 메인 엔티티를 먼저 로드하고, 관련된 엔티티는 실제로 접근할 때 로드됩니다.
