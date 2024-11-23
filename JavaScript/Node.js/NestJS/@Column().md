- @Column() [[데코레이터(Decorator)]] [[클래스(class)]]는 특장 [[엔티티(Entity)]]의  필드([[열(Column)]])을 나타내는 데 사용된다.

- 즉, [[테이블(Table)]]의 필드([[열(Column)]])를 생성할 수 있다.

- 특수 컬럼 [[데코레이터(Decorator)]]로 [[@CreateDateColumn()]], [[@UpdateDateColumn()]], [[@DeleteDateColumn()]], [[@VersionColumn()]]가 있다.


## @Column()의 옵션

### type

- ColumnType으로 필드([[열(Column)]])의 타입, varchar, text, int, bool 등 필드([[열(Column)]])의 타입을 나타낸다.
### name

- string으로 [[데이터베이스(DataBase)]]에 저장될 필드([[열(Column)]])의 이름이다.
- 기본값은 [[속성(Property)]]의 이름을 따른다.
### nullable

- boolean으로 [[null]]값이 가능한지 여부이다.
- 기본적으로 false이다.
### update

- boolean으로 업데이트 가능여부이다.
- false일 경우 저장 후 업데이트 불가하다.
- 기본값은 true이다.
### select

- boolean으로 [[쿼리(Query)]] 실행 시 [[속성(Property)]]을 가져올지 결정한다.
- false일 경우 가져오지 않는게 기본이다.
### default

- string으로 필드([[열(Column)]])의 기본값이다.
### unique

- boolean으로 unique constraint 적용여부이다.
- 기본 false이다.
### enum

- string[]으로 필드([[열(Column)]])에 입력 가능한 값을 enum으로 나열한다.
### array

- boolean으로 필드([[열(Column)]])에 array 타입으로 생성한다.
- 예를 들어 int[]이다.

