- 스프링 부트 [[JPA(Java Persistence API)]] 에서 [[엔티티(Entity)]]에 해당하는 파일에 [[@Entity]] @Table [[어노테이션(Annotation)]]을 사용할 수 있는데 일단 @Entity는 필수로 들어가야 하고 @Entity만 사용했을 경우 DB와 연결 시 테이블명은 클래스명과 동일하게 설정된다.

- 즉 @Entity 어노테이션을 사용한 상태에서 클래스명이 CrudEntity일 경우 DB에서 CrudEntity [[테이블(Table)]]로 연결된다는 거다.
- @Entity(name = "엔티티명") 으로 테이블명을 지정해 줄 경우에는 이후 EntityManager 등을 이용해 쿼리를 사용할 경우 createQuery("select .. from 엔티티명") 이렇게 사용할 수 있게 된다.


## @Table의 속성
### 1. name

- 매핑할 테이블의 이름을 지정한다.
### 2. catalog

- DB의 catalog를 맵핑한다.
### 3. schema 

- DB 스키마와 맵핑한다.
### 4. uniqueConstraint 

- [[DDL(Data Definition Language)]] [[쿼리(query)]]를 작성할 때 제약 조건을 생성한다.
## 예시

```java
@Entity
@Table(name = "sample_member")
public class CrudEntity {
	@Id
	@Coulmn(nullable = false, unique = true)
	private String name;
	
	@Coulmn(nullable = false)
	private int age;
}
```

- 반면 @Table의 경우에는 외부에서 호출하는 용도가 아닌 실제 DB에 붙을 테이블명 어노테이션을 말하는데 @Entity와 조합을 해서 사용해보면 위 코드와 같이 @Entity @Table(name = "테이블명")으로 지정을 해 놓을 경우createQuery(select m from 엔티티클래스명)으로 호출을 해 주면 호출은 [[엔티티(Entity)]] [[클래스(Class)]]명으로 하지만실제 DB에는 테이블명의 테이블로 붙게 된다.

- 또한 위의 @Table [[어노테이션(Annotation)]] 코드에서 [[@Index]] [[어노테이션(Annotation)]]를 추가적으로 붙일 수 있다.