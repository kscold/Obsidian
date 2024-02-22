- JpaRepository는 ListCrudRepository와 ListPagingAndSortingRepository를 상속받은 Spring Data JPA에서 제공하는 [[인터페이스(Interface)]]이다.

- [[CRUD]](생성, 조회, 수정, 삭제) 기능 뿐만 아니라 엔티티를 페이지 단위로 조회 및 정렬하는 기능과 [[JPA(Java Persistence API)]]에 특화된 여러 기능 등을 제공한다.

- JPARepository [[인터페이스(Interface)]]를 [[상속(Inheritance)]]받는 인터페이스를 정의하면, 해당 인터페이스를 구현하는 클래스는 JPA에서 제공하는 메서드들을 사용할 수 있다.
- 데이터베이스의 추가, 조회, 수정, 삭제의 findAll(), findById(), save() 등의 메서드들을 사용할 수 있다.
- 따라서 제공되는 메서드들 이용하여 쉽고 간편하게 CRUD 조작을 할 수 있다.

- 즉, JpaRepository를 사용하면, 복잡한 [[JDBC(Java DataBase Connectivity)]]코드를 작성하지 않아도 간단하게 DB와의 데이터 접근 작업을 처리할 수 있다.

- JPARepository 인터페이스는 제네릭 타입을 사용하여 Entity클래스와 복합키를 사용하고 있다면 해당 Entity의 ID클래스를 명시한다.

- 이를 통해 해당 인터페이스를 상속받는 구현체는 Entity클래스와 ID클래스에 대한 정보를 알고 있어서, 런타임 시점에 적절한 쿼리를 생성하고 실행할 수 있다.

## JPARepository 사용법

- Entity 클래스 정의한다.
- JPARepository를 사용하여 액세스할 [[엔티티(Entity)]] [[클래스(Class)]]를 정의해야한다.

```java
@Getter @Setter
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private Integer age;
}
```

- JpaRepository [[인터페이스(Interface)]] 상속받는 인터페이스 생성한다.
- JpaRepository 인터페이스를 상속받아, 커스텀한다.

```java
public interface UserRepository extends JpaRepository<User, Long> {

    @Query("SELECT u FROM User u WHERE u.age >= :age")
    List<User> findByAgeGreaterThanEqual(@Param("age") Integer age);
}
```

- Spring [[Bean]] 등록한다.
- JpaRepository를 사용하여 데이터 액세스를 수행하는 EntityManager가 필요하므로, JpaRepository를 사용하는 클래스는 빈으로 등록되어야 한다.

```java
@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;
    // ...
}
```

- JPARepository method 사용한다.
- JPARepository 인터페이스에 정의된 메서드를 호출하거나 구현한 메서드를 호출한다.

```java
@Service
public class UserService {

    @Autowired
    private UserRepository userRepository;
	
    public User saveUser(User user) {
        return userRepository.save(user);
    }
	
    public List<User> getAllUsers() {
        return userRepository.findAll();
    }
	
    public Optional<User> getUserById(Long id) {
        return userRepository.findById(id);
    }
	
    public void deleteUser(Long id) {
        userRepository.deleteById(id);
    }
    public List<User> getUsersByAge(Integer age) {
        return userRepository.findByAgeGreaterThanEqual(age);
    }
}
```


## JPARepository Method

### CRUD 메서드

- JPARepository는 [[CrudRepository]] [[인터페이스(Interface)]]를 [[상속(Inheritance)]]받는다.
- count, delete, deleteAll, deleteAllById, deleteById, existsById, findById, save 등의 기본적인 메서드를 제공한다.
### List CRUD 메서드

- JPARepository는 ListCrudRepository인터페이스도 상속받는다.

#### findAll  

- [[엔티티(Entity)]] [[클래스(Class)]]에 대한 모든 데이터를 조회한다.
- 반환값은 List 형태이며, T는 엔티티 클래스이다.
- 모든 데이터를 조회하므로, 대용량의 데이터를 조회할 때는 성능 이슈가 발생할 수 있다.

#### findAllById  

- 주어진 ID에 해당하는 엔티티를 조회한다.
- 매개변수로는 ID의 Iterable 객체를 전달한다.

#### [[List]]`<T>` findAllById([[Iterable]]`<ID>` ids)

- 이 메서드를 사용하면, ID의 Iterable 객체를 전달하여 여러 개의 엔티티를 조회할 수 있다.

#### saveAll  `<S extends T>` [[List]]`<S>` saveAll([[Iterable]]`<S>` entities)

- 주어진 엔티티를 모두 저장한다.
- 여러 개의 엔티티를 한 번에 저장할 수 있다. 
- 반환값으로 저장된 엔티티의 Iterable 객체를 반환하므로, 저장 결과를 확인할 수 있다.

  

3. Query Creation 메서드

JPARepository는 QueryByExampleExecutor 인터페이스도 상속받습니다.

> [SPRING DOCS : QueryByExampleExecutor](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/repository/query/QueryByExampleExecutor.html)

QBE는 Spring Data JPA에서 제공하는 기능으로, 동적으로 쿼리를 생성할 수 있습니다.  
QueryByExampleExecutor는 QBE를 구현한 인터페이스입니다.

- findOne  
    `<S extends T> Optional<S> findOne(Example<S> example)`  
    주어진 조건에 맞는 엔티티 하나를 반환합니다.  
    
- findAll  
    `<S extends T> Iterable<S> findAll(Example<S> example)`  
    주어진 조건에 해당하는 모든 엔티티를 반환합니다.  
    
- findBy  
    `<S extends T,R> R findBy(Example<S> example, Function<FluentQuery.FetchableFluentQuery<S>,R> queryFunction)`  
    주어진 조건에 맞는 쿼리를 생성하여 쿼리 실행 결과를 반환하는 메서드입니다.
    
    ```java
    Example<User> example = Example.of(new User(null, null, 30));
    List<User> users = userRepository.findBy(example, query -> query.where(QUser.user.age.goe(30)).orderBy(QUser.user.name.asc()));
    ```
    
- exists  
    `<S extends T> boolean exists(Example<S> example)`  
    주어진 조건에 맞는 엔티티가 존재하는지 확인합니다.  
    
- count  
    `<S extends T> long count(Example<S> example)`  
    주어진 조건에 해당하는 엔티티의 수를 반환합니다.
    
    *Example객체 : 원하는 쿼리조건을 기반으로 만들어진 엔티티 객체
    

  

4. 영속성 컨텍스트 관련 메서드

- flush()  
    현재의 트랜잭션에서 관리되고 있는 영속성 컨텍스트에 있는 변경 내용을 데이터베이스에 즉시 반영하는 메소드입니다. 모든 변경 내용이 즉시 데이터베이스에 반영되므로, 데이터베이스와의 동기화 작업이 매우 빈번하게 발생할 수 있습니다. 이는 성능상 이슈를 유발할 수 있으므로, 적절한 상황에서만 메소드를 사용하는 것이 좋습니다.

```java
public void updateUser(Long userId, String email) {
        User user = userRepository.findById(userId).orElseThrow(() -> new IllegalArgumentException("Invalid user id"));
        user.setEmail(email);
        userRepository.flush(); // 영속성 컨텍스트의 변경 내용을 즉시 데이터베이스에 반영합니다.
    }
```

- saveAndFlush  
    `<S extends T> S saveAndFlush(S entity)`  
    엔티티를 저장하고, 영속성 컨텍스트의 변경 내용을 즉시 데이터베이스에 반영합니다.  
    
- deleteAllInBatch  
    모든 엔티티를 데이터베이스에서 삭제합니다. 이 메소드는 deleteAll() 메소드와 달리 영속성 컨텍스트를 거치지 않고 바로 데이터베이스에서 삭제하기 때문에, 대규모 데이터 삭제 작업에 적합합니다.