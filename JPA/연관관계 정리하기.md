# 연관관계 정리하기 (단방향 / 양방향  @OneToMany)



## 양방향 @OneToMany

![image-20241207160029062](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20241207160029062.png)

- **항상 부모 측에서 자식측으로 전이를 사용해야 합니다**.
  - 예시로 학교와 학생이 있다면 
  - 학교가 없어진다면 학생들도 사라지겠지만 학생들이 사라진다고해서 학교가 없어지지는 않습니다.
  - 즉 엔티티의 상태 전환이 부모 측 엔티티에서 자식 측 엔티티로 전파되어야 합니다.



예시

```  java
@Entity
public class Author {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    
    // cascade 영속성 전이, 여기서는 부모 엔티티의 영속성을 자식엔티티에 전달한다.,
    //  CascadeType.ALL이기 때문
    // orphanRemoval 
    //  JPA에서는 부모 엔티티와 연관관계가 끊어진 자식 엔티티를 자동으로 삭제하는 기능을 제공하는데 이를 고아 객체 제거
    @OneToMany(mappedBy = "author", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<Book> books = new ArrayList<>();
    
    // Getter, Setter, 유틸리티 메서드
    public void addBook(Book book) {
        books.add(book);
        book.setAuthor(this);
    }
    
    public void removeBook(Book book) {
        books.remove(book);
        book.setAuthor(null);
    }
}

@Entity
public class Book {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String title;
    
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "author_id")
    private Author author;
    
    // Getter, Setter
}

```



- 생성

```  java
public void createAuthorWithBooks() {
    Author author = new Author();
    author.setName("John Doe");

    Book book1 = new Book();
    book1.setTitle("Book A");
    author.addBook(book1);

    Book book2 = new Book();
    book2.setTitle("Book B");
    author.addBook(book2);

    // author를 save하면, cascade 설정 덕분에 book들도 자동 영속화됨
    authorRepository.save(author);
}
```

- 조회

``` java
@Transactional
public void readAuthorAndBooks(Long authorId) {
    Author author = authorRepository.findById(authorId)
                                    .orElseThrow(() -> new RuntimeException("Author not found"));
    List<Book> books = author.getBooks();
    // 트랜잭션 내에서 접근 시 LAZY 로딩된 books 리스트도 사용 가능
}
```

- 수정

```java
@Transactional
public void addNewBookToAuthor(Long authorId, String newBookTitle) {
    Author author = authorRepository.findById(authorId)
                                    .orElseThrow(() -> new RuntimeException("Author not found"));

    Book newBook = new Book();
    newBook.setTitle(newBookTitle);

    author.addBook(newBook);

    // author를 save하면 cascade로 인해 newBook도 저장됨
    authorRepository.save(author);
}
```

- 기존 작가에서 책을 제거함.

``` java
@Transactional
public void removeBookFromAuthor(Long authorId, Long bookId) {
    Author author = authorRepository.findById(authorId)
                                    .orElseThrow(() -> new RuntimeException("Author not found"));

    Book book = author.getBooks().stream()
                      .filter(b -> b.getId().equals(bookId))
                      .findFirst()
                      .orElseThrow(() -> new RuntimeException("Book not found"));

    author.removeBook(book);

    // orphanRemoval = true이므로 커밋 시 book은 자동 삭제됨
    authorRepository.save(author);
}
```

- 삭제 

```  java
@Transactional
public void deleteAuthor(Long authorId) {
    if (!authorRepository.existsById(authorId)) {
        throw new RuntimeException("Author not found");
    }
    authorRepository.deleteById(authorId);
    // cascade와 orphanRemoval 덕분에 관련된 book들도 삭제됨
}
```



## 단방향 @OneToMany

- 부모 자식 연관관계를 관리하기 위한 연결 테이블을 생성합니다.

  ![image-20241207161532566](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20241207161532566.png)

- 자식 엔티티가 부모 엔티티를 직접 참조하지 않기 때문에 별도의 연결 테이블을 생성함

  ![image-20241207160821845](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20241207160821845.png)

  - 이러한 연결 테이블의 생성으로 인한 비효율이 발생합니다.
    - 메모리 사용 증가 및 성능 저하의 가능성
      - 두 개의 외래키 열에 대한 인덱싱
      - 연결 테이블 자체의 오버헤드
      - 데이터베이스 오버헤드가 발생함
    - 3개의 테이블로 인한 쿼리 복잡성 증가
  - Insert나 delete가 작동되어질때
    - 불필요한 삭제와 삽입 작업이 발생합니다.
    - 부모 엔티티가 자식 엔티티와의 관계를 직접 관리하지 않기 때문입니다.
      - 즉 자식 엔티티의 상태를 정확히 파악하기 위한 방법이 제한적입니다.

- 연결 테이블을 생성하지 않는 법

  - @JoinColumn을 사용합니다.

    - @JoinColumn을 지정하면 @OneToMany 연관관계가 자식 테이블 외래키를 제어할 수있음을 Hibernate에 지시합니다.

    - @JoinColumn을 추가하면 단방향 @OneToMany 보다는 이점을 제공할 수 있지만 추가 update문은 여전히 성능저하를 가져옵니다.



### Cascade란?

-  A라는 엔티티에 어떤 작업을 수행했을 때, 그 작업이 연관된 B라는 엔티티도 이루어져야 한다

### Cascade 사용시, 대부분의 경우 제거에 대한 전이는 좋은 생각이 아닙니다.

- 예상치 못한 결과를 불러올 수있기 때문입니다.(다른 엔티티와 연관되었을 수도 있기 때문)

- CascadeType.ALL 및 CascadeType.REMOVE 사용을 피하고 명시적으로 CasacdeType.PERSIST

  CascadeType.MERGE를 사용해야 합니다.



### N+1 문제

- **연관 관계에서 발생하는 이슈로 연관 관계가 설정된 엔티티를 조회할 경우에 조회된 데이터 갯수(n) 만큼 연관관계의 조회 쿼리가 추가로 발생하여 데이터를 읽어오게 된다**

- @OneToMany는 기본전략으로 지연로딩을 가져갑니다.

#### 원인의 분석

> - JPA/Hibernate 환경에서 @OneToMany 연관관계를 기본 설정(지연로딩)으로 사용하면 N+1 문제가 자주 발생합니다.
> - 보통 "1" 측 엔티티를 조회할 때, 해당 **엔티티는 영속성 컨텍스트의 1차 캐시에 저장**됩니다. 
> - 하지만 "N" 측 엔티티들은 기본적으로 **지연 로딩을 사용하는 프록시 객체로 표현**되며, 이들은 즉시 조회되지 않기 때문에 1차 캐시에 포함되지 않습니다.
> - "1" 측 엔티티의 컬렉션을 순회할 때마다, **지연 로딩된 각 "N" 측 엔티티를 실제로 가져오기 위해 데이터베이스를 추가로 조회하는 쿼리**가 실행됩니다. 
> - 이렇게 하면 N개의 관련 엔티티에 대해 N번의 추가 쿼리가 발생하고, 최초 "1" 측 엔티티를 가져오는 1번의 쿼리와 합쳐져 결국 N+1 쿼리 문제가 발생하게 됩니다.



![image-20241207165043286](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20241207165043286.png)

- 지연로딩으로 가져오기에 N측은 proxy 객체로 가져오게 됩니다.
  - proxy 객체는 즉시 조회되어지지 않기에 1차 캐시에 포함되어지지 않습니다.

![image-20241207165157977](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20241207165157977.png)

- N + 1의 문제가 발생합니다.
  - 즉 '1'측 엔티티의 컬랙션을 순회할 때마다, 지연 로딩된 각 'N' 측 엔티티를 실제로 가져오기 위해 추가로 데이터베이스를 조회하는 쿼리가 발생합니다.
  - 이렇게 하면 N개의 관련 엔티티에 대해서 N번의 추가 쿼리가 발생하고, 최초 '1'측 엔티티를 가져오는 1번의 쿼리와 합쳐져서 N+1 쿼리문제가 발생하게 됩니다.

![image-20241207201601777](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20241207201601777.png)

- 이러한 문제를 해결하기 위해서 fetch join을 사용할 수 있습니다.
  - fetch join이란?
    - 연관된 엔티티나 컬랙션을 한 번에 같이 조회하는 기능입니다.
    - 즉 '1'측 엔티티를 조회할때 연관된 엔티티도 영속화 시키는 것입니다.



