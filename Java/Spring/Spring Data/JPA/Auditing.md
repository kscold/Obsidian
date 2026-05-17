- 데이터를 관리하는데 있어서 생성시점, 수정시점 등 언제 데이터가 변경되었는지 기록해 두는 것은 매우 중요하다.
- 'audit' 는 심사, 감사하다는 뜻이다.

- 그리고 이러한 것들은 한 [[엔티티(Entity)]]에 국한된 것이 아니라 모든 [[엔티티(Entity)]]에 적용된다.
- 중복을 편하게 해결하기 위해 Spring Data는 [[엔티티(Entity)]]를 생성하거나 변경한 시점을 추적하기 위한 것들을 제공한다.

- 이 기능을 이용하면 데이터를 생성하거나 수정할 때 따로 로직을 구현하지 않아도 자동적으로 시간 데이터를 관리해준다.





## Spring Auditing의 [[어노테이션(Annotation)]]

- 데이터를 저장할 때 '생성된 시간 정보'와 '수정된 시간 정보'는 여러모로 많이 사용되고 또 중요하다.

- [[JPA(Java Persistence API)]]를 사용하면서 [[@CreatedDate]], [[@LastModifiedDate]]를 사용하여 생성된 시간 정보, 수정된 시간 정보를 자동으로 저장할 수 있다.

 - createAt, updatedAt 값이 계속 null로 들어가던 문제는 main method가 실행되는 Application class에 @EnableJpaAuditing 어노테이션을 적용하지 않기 때문이다.

- JPA Auditing 기능과 @CreatedBy, @LastModifiedBy 를 사용하여, 데이터가 생성되거나 수정될 때 유저의 ID가 DB에 저장되게 기능을 구할 수 있다.