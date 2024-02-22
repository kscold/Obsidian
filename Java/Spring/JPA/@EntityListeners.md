- @EntityListeners는 [[엔티티(Entity)]]를 DB에 적용하기 전, 이후에 커스텀 콜백을 요청할 수 있는 [[어노테이션(Annotation)]]이다.

- @EntityListeners의 인자로 커스텀 콜백을 요청할 클래스를 지정해주면 되는데, [[Auditing]]을 수행할 때는 [[JPA(Java Persistence API)]] 에서 제공하는 AuditingEntityListener.class를 인자로 넘기면 된다.

- 해당 클래스에 Auditing 기능을 포함