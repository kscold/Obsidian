- @MappedSuperClass은 [[엔티티(Entity)]]의 공통 매핑 정보가 필요할 때 주로 사용한다.
- 즉, [[부모 클래스(super class)]]([[엔티티(Entity)]])에 [[Java/필드(Field)|필드(Field)]]를 선언하고 단순히 속성만 받아서 사용하고싶을 때 사용하는 방법이다.

- 보통 개발자는 BaseEntity를 생성하고 [[Auditing]] 기능이 필요한 [[엔티티(Entity)]] 클래스에서 사용할 것이기 때문에 @MappedSuperClass [[어노테이션(Annotation)]]을 사용하는 것이다.