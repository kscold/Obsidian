- [[값 타입 컬렉션]]을 매핑할 때 사용합니다.
- [[관계형 데이터베이스(Relational DataBase)]]에는 컬렉션([[Collection]])과 같은 형태의 데이터를 컬럼([[Spring/SQL/필드(Field)|필드(Field)]])에 저장할 수 없기 때문에, 별도의 [[테이블(Table)]]을 생성하여 컬렉션([[Collection]])을 관리해야 한다.

- 이때 해당 [[Spring/SQL/필드(Field)|필드(Field)]]가 컬렉션([[Collection]]) [[객체(Object)]]임을 [[JPA(Java Persistence API)]]에게 알려주는 [[어노테이션(Annotation)]]이 @ElementCollection이다.

- ElementCollection의 장점은 종속적이기 때문에 데이터를 삭제 할 때 [[엔티티(Entity)]]만 지우면 ElementCollection도 따라서 같이 지워진다.

- [[@Entity]]가 아닌 [[원시 타입(Primitive Type)]]이나 Embeddable Class로 정의된 컬렉션[[@Embedded]])을 [[테이블(Table)]]로 생성하며 [[일대다(OnetoMany)]] 관계로 다룬다.


## @ElementCollection의 예시

- 아래에는 스터디그룹과 멤버 구성을 관리하는 Entity이다. 
- 스터디 그룹에 대해 멤버는 [[일대다(OnetoMany)]] 관계로 구성되며, 멤버는 다양한 스터디 그룹에 참여할 수 있다.

- StudyGroup entity : Entity, 스터디 그룹을 관리
    - id : PK, 스터디 그룹을 대표하는 값
    - topicTags : Basic Collection, 스터디 주제
    - GroupMember : Embedded Collection, 스터디 멤버


```java
@Entity
public class StudyGroup {    
	@Id     
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private Long id;     // Basic type    
	
	@ElementCollection(fetch = FetchType.LAZY)
	private Set<String> topicTags = new HashSet<String>(); // Embedded type    
	
	@ElementCollection
	@CollectionTable(name="study_group_member", joinColumns = @JoinColumn(name= "study_group_id", referencedColumnName = "id"))    
	
	private List<StudyGroupMember> members = new HashSet<StudyGroupMember>();
}
```

```java
@Embeddablepublic 
class StudyGroupMember {    
	private UUID memberId;    
	private Boolean isOwner;
}
```



### @ElementCollection [[@OneToMany]]의 차이점

- 언뜻 [[연관 관계(Relationships)]]만 봤을 때에는 @ElementCollection과 [[JPA(Java Persistence API)]] [[@Entity]]의 [[@OneToMany]]가 유사하게 느껴진다.
- 이 둘의 차이점은 간단하게 알아보자.

#### @ElementCollection

- 연관된 부모 [[엔티티(Entity)]] 하나에만 연관되어 관리된다. (부모 Entity와 독립적으로 사용이 불가능하다.)
- 항상 부모와 함께 저장되고 삭제되므로 cascade 옵션은 제공하지 않는다. (cascade = ALL인 셈이다.)
- 부모 Entity Id와 추가 컬럼(basic or embedded 타입)으로 구성된다.
- 기본적으로 식별자 개념이 없으므로 컬렉션 값 변경 시, 전체 삭제 후 새로 추가한다.
 
#### @Entity 연관 ([[@OneToMany]] / @ManyToMany)

- 다른 [[엔티티(Entity)]]에 의해 관리될 수도 있다.
- [[JOIN]] [[테이블(Table)]]이나 컬럼([[Spring/SQL/필드(Field)|필드(Field)]])은 보통 id만으로 연관을 맺는다.