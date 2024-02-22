- 데이터(data)는 각 항목에 저장된 값을 의미한다.
- 구조화된 데이터는 하나의 [[테이블(Table)]]로 표현할 수 있다.

- 사전에 정의된 테이블을 Relation이라고도 부르기 때문에, [[테이블(Table)]]을 사용하는 데이터베이스를 관계형 데이터베이스(Relational database)라고 한다.


## 관계형 데이터 베이스 구조

- 관계형 데이터 베이스는 "데이터 베이스 > 테이블 > 레코드, 칼럼 > 데이터"의 계층적 구조를 가진다.
- 관계형 데이터 베이스 용어는 SQL 용어와 혼용하여 쓰이기 때문에 다소 복잡한 편이다.

### 테이블/관계(table/relation) 

- [[레코드(Record)]]와 [[속성(Attribute)]]의 모임을 의미한다.

### [[레코드(Record)]]/튜플/로우/행(record/tuple/row)

- 레코드, 튜플, 로우, 행이 전부 같은 말을 의미한다.
- 하나의 항목을 대표하는 데이터이다. ( 예) 이름)
    
### [[속성(Attribute)]]/칼럼/[[Spring/SQL/필드(Field)|필드(Field)]]/열(attribute/column/field)

- 속성, 칼럼, 필드, 열이 전부 같은 의미로 혼용된다.
- 항목을 구분하는 데이터이다. ( 예) 나이, 성별)

## [[JPA(Java Persistence API)]]에서 다양한 연관관계

- [[엔티티(Entity)]]들은 서로 다양한 연관관계를 맺을 수 있다.

|연관관계|JPA Annotation|
|---|---|
|1:1 |@OneToOne |
|1:N |[[@OneToMany]] |
|N:1 |[[@ManyToOne]] |
|N:M |@ManyToMany |

- N:M 연관관계는 RDB에서 일반적인 방법으로 표현할 수 없어서 중간 [[테이블(Table)]]이 생기게 된다.
- 따로 중간 [[엔티티(Entity)]]([[테이블(Table)]])를 만들어서 1:N, N:1 관계로 분해하지 않으면 관리가 힘들어지기 때문에 사용을 권장하지 않는다.

- 1:1 연관관계도 사용 시 객체지향적으로 개발할 수 있다는 점 등 장점이 있지만, 단점도 존재하기에 주의해서 사용해야 된다.

## @OneToMany, @ManyToOne 편의 옵션

### fetch(프록시 옵션)

- 해당 [[객체(Object)]]를 DB에서 조회할 때, 연관관계에 있는 [[엔티티(Entity)]]의 정보를 언제 같이 끌어올 지에 대한 옵션이다.
#### 1. Lazy Fetch

- 연관관계에 있는 [[엔티티(Entity)]]에 접근할 때, DB에 [[쿼리(query)]]를 날려 [[엔티티(Entity)]]를 조회하게 된다.
- 접근하지 않는 경우, 쿼리가 발생하지 않는다.
#### 2. Eager Fetch

- 상대 [[엔티티(Entity)]]의 조회 여부와 상관없이, [[쿼리(query)]]가 발생하게 된다.
- @OneToMany의 기본값은 Lazy Fetch이며, @ManyToOne의 기본값은 Eager Fetch이다.

- Eager Fetch, Lazy Fetch 상관 없이 단건 조회가 아닌 컬렉션 조회에서 N+1 문제가 발생할 수 있다.
- Eager Fetch는 조회 여부와 상관없이 쿼리가 발생하기 때문에, 더 잘 보이는 차이가 있을 뿐이다.

### cascade(영속성 전파)

- CascadeType으로 6가지를 줄 수 있다.
- PERSIST, MERGE, REMOVE, REFRESH, DETACH와 모든 옵션을 줄 수 있는 ALL이다.

- 영속성 전파를 설정하게 되면, [[객체(Object)]]에 해당 작업이 이루어질 때, 자식 [[엔티티(Entity)]]에도 작업이 전파된다.

- 예를 들어, 유저의 posts에 cascadeType으로 PERSIST가 걸려있으면, 유저 객체만 저장해도 글 객체도 저장된다.

### orphanRemoval(고아 객체 관리)

- orphanRemoval 옵션은 [[@OneToMany]]에만 존재하는 옵션이다.
- orphanRemoval 옵션은 연관관계가 끊긴 [[엔티티(Entity)]]에 대해서 REMOVE 작업을 진행하고 전파할 지에 대한 옵션이다.

```java
user.getPosts().remove(post);
post.setUser(null);
```

- 예를 들어 위 코드를 실행하면 유저의 post 리스트에 해당 [[객체(Object)]]가 존재하지 않아서 연관관계가 끊기게 된다.
- 이 때, DB로 삭제하는 [[쿼리(query)]]가 발생한다.

- 주의해야 될 점은 PERSIST의 cascade가 적용되어야, orphanRemoval이 제대로 작동한다.
- [[JPA(Java Persistence API)]]가 컬렉션에 있는 엔티티를 추적해야 되기 때문이다.

- 착각하면 안되는 것이, cascade의 REMOVE와는 조금 다르다.
- cascade의 REMOVE은 해당 엔티티가 삭제되었을 때, 그 삭제 작업을 연관관계에 있는 엔티티까지 전파할 것인지 조정하는 옵션이다.

- 여기서 user는 삭제되지 않았기 때문에 관련이 없는 것이다.

### optional(Null 관리)

- optional 옵션은 [[@ManyToOne]]에만 존재하는 옵션이다.
- 해당 옵션은 [[외래 키(Foreign Key)]](FK) 칼럼([[Spring/SQL/필드(Field)|필드(Field)]])에 Null 여부를 설정한다.

- 기본값은 true이며, false인 경우, FK에 Null을 허용하지 않는다.

## 양방향과 단방향 연관관계

- [[@OneToMany]] 기준으로 설명하면 단방향(unidirectional)은 상대 엔티티에 @ManyToOne이 없는 경우, 양방향(bidirectional)은 상대 엔티티에 @ManyToOne이 있는 경우이다.

- [[@ManyToOne]] 기준으로 설명드리자면 단방향(unidirectional)은 상대 엔티티에 @OneToMany가 없는 경우, 양방향(bidirectional)은 상대 엔티티에 @OneToMany가 있는 경우이다.

- @OneToMany 양방향과 @ManyToOne 양방향은 기준이 다를 뿐, 차이가 없다.
- 단뱡향이든 양방향이든 @OneToMany [[어노테이션(Annotation)]]을 달고 있는 [[엔티티(Entity)]]가 부모 엔티티가 된다.
- 즉, [[외래 키(Foreign Key)]](FK)를 들고 있는 쪽이 자식 엔티티가 된다.