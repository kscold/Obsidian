
- Spring Data [[JPA(Java Persistence API)]]에서 제공하는 [[CrudRepository]]에서 값을 저장하는 save [[메서드(Method)]]이다.

## JPA에서의 save 메서드 구현체 코드

```java
// SimpleJpaRepository.java

@Transactional
@Override
public <S extends T> S save(S entity) { // 엔티티(T)를 추상 메서드로 상속받아 S를 정의
	Assert.notNull(entity, "Entity must not be null.");
	
	if (entityInformation.isNew(entity)) { // 1 엔티티가 완전 처음 들어왔을 때는 insert(create)
		em.persist(entity); // 2
		return entity; // 3
	
	} else { // 4 엔티티가 이미 존재할 경우에는 merge(update)
		return em.merge(entity); // 5 
	}
}
```

- JPA 내부에서 1번의 isNew 메서드를 통해 매개변수로 들어온 [[엔티티(Entity)]]가 새로운 것인지, 이미 존재하는 entity인지 확인을 한다.
	- [[참조 타입(Reference Type)]] `[String, Long etc..]` 일 경우 null인지 확인한다.
	- [[원시 타입(Primitive Type)]] `[int, long, char etc..]` 일 경우 값이 0인지 확인한다.

- 따라서 [[@Id]]생성 방식이 [[@GeneratedValue]] 일 경우에는 2번의 em.persist를 하기 전까지 해당 멤버 변수가 null 혹은 0이기 때문에 새로운 entity임을 인지하고 if문을 타서 2, 3번을 순차적으로 수행한다.
- 3번의 return entity; 와 같이 save를 호출한 곳에서는 entity를 사용할 수 있으며, return을 통해 넘어간 entity에는 원래는 null 이었을 @Id의 멤버 필드가 JPA의 @GeneratedValue 전략에 맞춰 값이 채워져 있는 것을 확인할 수 있다.

- 그러나 만약 @Id가 @GeneratedValue가 아닌 사용자가 직접 입력을 받는 변수일 경우는 어떻게 될까?

- 아래 예시 코드와 같이 Member 객체가 있다고 가정해보자

```java
@Entity
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class Member {
    @Id
    @Column
    private String email;
    
    private String name;
    
    private Member(String email, String name) { // 생성자 정의
        this.email = email;
        this.name = name;
    }
    
    public static Member createMember(String email, String name) { // 생성자 메서드 호출
	    // static이 붙었으므로 인스턴트를 만들지 않고 바로 호출가능
        return new Member(email, name);
    }
}
```

- 위 Member 객체처럼 회원가입 시 사용자가 입력한 이메일을 PK로 설정했을 경우에 JPA 구현체 내부에서 1번 isNew(entity) 메서드가 String email 변수를 확인하고, null이 아닌 값이 채워져 있기 때문에 if문이 아닌 else 구문으로 이동하게 된다.
- 그리하여 5번의 em.merge(entity)를 수행하게 되는데, 여기서 실제로 회원가입을 통해 입력한 이메일이 실제로 존재하는지 확인하기 위한 [[SELECT]] [[쿼리(query)]] 한 번을 보내게 된다.

- [[SELECT]] [[쿼리(query)]]를 한 번 날림으로써 실제로 해당 이메일이 DB에 존재하는지, 존재하지 않는지 확인한다.

- DB에 있을 경우에는 [[엔티티(Entity)]]에 입력된 정보로 merge가 발생한다.
- DB에 없을 경우에는 새로운 [[엔티티(Entity)]]이므로 [[INSERT]] [[쿼리(query)]]가 발생한다.

- 밑의 코드는 클래스의 엔티티 테스트코드 예시이다.
```java
@SpringBootTest
class MemberTest {
	@Autowired
	private MemberRepository memberRepository;
	
	@Test
	public void memberSaveTest throws Exception {
		String email = "wangtak@gmail.com";
		String name = "왕탁이";
		Member member = Member.createMember(email, name);
		
		memberRepository.save(member);
	}
}
```

- 밑의 이미지는 실제 쿼리 결과 로그이다.

![](https://blog.kakaocdn.net/dn/TIckD/btrmbWhMAQ1/rz5xnmhZI4hlo7ttB480c0/img.png)

- 이처럼 순수하게 save를 원하는 상황에서 SELECT라는 불필요한 쿼리가 날아가게 되므로 잘못 설계한 도메인이라 할 수 있다.
- 이와 같은 방법을 쉽게 해결하려면 [[엔티티(Entity)]]자체의 @Id 값을 @GeneratedValue로 설정하면 되겠지만, 부득이하게 사용자의 입력에 의존하는 @Id를 설계해야 할 경우도 있다.

- 이 경우에는 다음과 같이 해결할 수 있습니다.

```java
@Entity
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class Member extends BaseEntity implements Persistable<String> {
    @Id
    @Column
    private String email;
    
    private String name;
    
    private Member(String email, String name) { // 생성자 저의
        this.email = email;
        this.name = name;
    }
    
    public static Member createMember(String email, String name) {
        return new Member(email, name);
    }
    
    @Override
    public String getId() {
        return email;
    }
    
    @Override
    public boolean isNew() {
        return getCreatedDate() == null; // 객체가 만들어진 시간 baseEntity에 존재함
    }
}
```

- 위 코드에는 `JPA Auditing[BaseEntity]`이 적용되어있다.

- 눈여겨봐야 할 부분은 바로 implements Persistable `<String>`이란 코드가 추가된 것이다.
- Persistable`<Id의 Type>` [[인터페이스(Interface)]]를 상속하게 되면, [[JPA(Java Persistence API)]] 구현체 내부에서 작동했던 isNew()를 개발자가 [[오버라이딩(Overriding)]]하여 비교 대상을 직접 지정할 수 있다.

- getId() 메서드는 @Id의 멤버 변수를 return 하도록 구현한다.
- isNew() 메서드는 객체가 만들어진 시간의 null 여부에 따라 새로운 entity인지 아닌지를 구분하라고 지정해준다.

- Member 테스트 코드를 수정하지 않고 똑같이 실행했을 경우 다음과 같은 결과를 확인할 수 있다.

![](https://blog.kakaocdn.net/dn/bmtMKh/btrmcLmR4eV/bkra2cqfwyZQ9jmmqqkks1/img.png)

- 위의 이미지는 Auditing, Persistable 구현 후 Query 결과이다.

- 이처럼 Auditing과 Persistable<>을 구현하게 되면 불필요한 SELECT 쿼리를 날리지 않고 INSERT 쿼리 한 번만 날리는 것을 확인할 수 있다.

