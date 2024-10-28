- [[TypeORM]]에서 [[일대다(OnetoMany)]] [[연관 관계(Relationships)]]를 나태낼 수 있는 [[데코레이터(Decorator)]]이다.

- A [[테이블(Table)]]의 [[행(Row)]](데이터) 하나와 B [[테이블(Table)]]의 [[행(Row)]](데이터) 여러개가 연결되는 관계이다.
- 사실상 [[@ManyToOne()]]과 세트이다.

- [[@ManyToOne()]]의 경우 관계정의가 필수적이지만, @OneToMany()의 경우 생략이 가능하다.
- 연관관계의 주인이 아니므로 @OneToMany()가 정의된 [[엔티티(Entity)]]의 [[테이블(Table)]]의 경우, 추가 필드([[열(Column)]])이 생기지 않는다.