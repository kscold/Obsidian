- [[연관 관계(Relationships)]]가 있는 [[엔티티(Entity)]]를 조회할 경우 지연 로딩으로 설정되어 있으면 [[연관 관계(Relationships)]]에서 종속된 [[엔티티(Entity)]]는 [[쿼리(query)]] 실행 시 [[SELECT]] 되지 않고 proxy [[객체(Object)]]를 만들어 [[엔티티(Entity)]]가 적용시킨다.

- 그 후 해당 proxy [[객체(Object)]]를 호출할 때마다 그때그때 [[SELECT]] [[쿼리(query)]]가 실행된다. 

- 추가적인 내용은 [[JPA(Java Persistence API)]]의 지연 로딩에 관련이 있다.

- [[연관 관계(Relationships)]]가 지연 로딩으로 되어있을 경우 fetch [[JOIN]]을 사용하여 여러 번의 [[쿼리(query)]]를 한 번에 해결할 수 있다.

- @EntityGraph는 Data JPA에서 fect [[JOIN]]을 [[어노테이션(Annotation)]]으로 사용할 수 있도록 만들어 준 기능이다.

## @EntityGraph를 사용한 연관관계 매핑

- 먼저 밑의 코드는 [[엔티티(Entity)]] [[클래스(Class)]]이다.

```java
package study.datajpa.entity;

import lombok.*;

import javax.persistence.*;

import static javax.persistence.FetchType.*;

@Entity
@Getter
@Setter
@NoArgsConstructor(access = AccessLevel.PROTECTED) // 기본생성자를 만들어 주는 기능
public class Member extends BaseEntity {
	
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "member_id")
    private Long Id;
    private String username;
    private int age;
	
    @ManyToOne(fetch = LAZY)
    @JoinColumn(name = "team1_id")
    private Team team;
	
    public Member( String username) {
        this.username = username;
    }
	
}
```

```java
package study.datajpa.entity;

import lombok.*;

import javax.persistence.*;
import java.util.ArrayList;
import java.util.List;

import static javax.persistence.CascadeType.ALL;

@Entity
@Getter
@Setter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class Team {
	
    @Id
    @GeneratedValue
    @Column(name = "team_id")
    private Long id;
    private String name;
	
    @OneToMany(mappedBy = "team", cascade = ALL, orphanRemoval = true)
    private List<Member> members = new ArrayList<>();
	
    public Team(String name) {
        this.name = name;
    }
}
```

- 밑의 코드는 [[테스트 메서드(Test Method)]]이다.

```java
   @Test
    public void basicCRUD() {
        Member member1 = new Member("member1");
        Member member2 = new Member("member2");
        memberRepository.save(member1);
        memberRepository.save(member2);
		
		
        List<Member> all = memberRepository.findAll();
        assertThat(all.size()).isEqualTo(2);
	}
```

- 실행 시 [[쿼리(query)]]를 확인해 보면 아래 코드처럼 지연로딩 의 경우 member 객체만 조회하고 team 객체는 proxy [[객체(Object)]]가 대체되어 들어간다.

![](https://blog.kakaocdn.net/dn/bQ7Qba/btq7iOSDYhv/mC8HnwryBbcRWnDv0BvZTk/img.png)


```java
	public interface MemberRepository extends JpaRepository<Member,Long>, MemberRepositoryCustom{
    
	@Override //기본 적으로 findAll 을 제공하기 때문에 Override 하여 재정의 후 사용 
	@EntityGraph(attributePaths = {"team"}) // DataJpa 에서 fetch 조인을 하기 위한 설정
    List<Member> findAll();
	
	}
```

- 위의 코드처럼 [[리파지터리(Repository)]]에 [[오버라이딩(Overriding)]]한 다음 @EntitiyGraph를 실행시켜 보면 team 엔티티를 함께 조회하는 것 을 확인할 수 있다.

![](https://blog.kakaocdn.net/dn/cgEfWT/btq7fcNKCGX/21DljSIPNrfbmdk8kGZSmK/img.png)

