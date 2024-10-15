- @Entity [[데코레이터(Decorator)]]는  특정 [[클래스(class)]]가 [[엔티티(Entity)]]로 사용됨을 알 수 있다.
- 즉, [[클래스(class)]]로 [[테이블(Table)]]을 관리할 수 있다.

- [[쿼리(Query)]]문으로 치면 [[CREATE]] TABLE [[테이블(Table)]]명 부분이다.


## [[엔티티(Entity)]] 선언 [[데코레이터(Decorator)]]

- [[NestJS]]에서 [[엔티티(Entity)]]를 선언할 때, @Entity() [[데코레이터(Decorator)]]와 같이 쓰이는 [[데코레이터(Decorator)]]는 다음과 같다.

### [[@Column()]]

- [[@Column()]] [[데코레이터(Decorator)]]를 통해 [[테이블(Table)]]의 필드([[열(Column)]])를 생성할 수 있다.

### [[@PrimaryGeneratedColumn()]]

-  [[@PrimaryGeneratedColumn()]] [[데코레이터(Decorator)]]를 통해 자동생성되는 고유한 ID 필드([[열(Column)]])를 생성할 수 있다.