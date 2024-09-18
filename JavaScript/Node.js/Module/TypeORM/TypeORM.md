



## [[연관 관계(Relationships)]]

- TypeORM의 [[데코레이터(Decorator)]]를 사용하면 관련 [[엔티티(Entity)]]에서 쉽게 사용 가능하며 다음과 같은 유형의 관계 설정이 가능하다.

 - [[일대일(OneToOne)]] - [[@OneToOne()]],  [[다대일(ManyToOne)]] - [[@ManyToOne()]], [[일대다(OnetoMany)]] - [[@OneToMany()]], [[다대다(ManyToMany)]] - [[@ManyToMany()]]로 표현할 수 있다.
 - 이런 TypeORM의 연관관계 설정 [[데코레이터(Decorator)]]는 모두 다 [[매개변수(parameter)]]는 type, inverseSide, option를 가진다.

### Option

- createForeignKeyConstraints : `boolean` false면 외래키 제약조건을 없앨수 있다.
- lazy : `boolean` true로 설정하면 관계를 프록시 객체로 바인딩한다.
- eager : `boolean` true로 설정하면 find*[[메서드(Method)]]를 사용하거나 QueryBuilder에서 관계가 항상 기본 [[엔티티(Entity)]]와 함께 로드된다.
- cascade : `boolean | ("insert" | "update" | "remove" | "soft-remove" | "recover")[]` true로 설정하면 케스케이드 옵션이 적용된다.
- onDelete : `"RESTRICT" | "CASCADE" | "SET NULL" ` 참조된 객체가 삭제될 때 [[외래 키(Foreign Key)]]의 동작 방식을 정의한다. (원본 유지, 같이 삭제,  NULL로 만듬)
- nullable : `boolean` 이 관계에서 null이 허용되는지 여부이다.
- orphanedRowAction : `"nullfy"|"delete"|"soft-delete"|"disable"` 관계 요소 없이 생성된 열이 있을 때 실제 일어날 일을 정의함 delete는 제거하며, soft-delete는 soft-deleted처리하며, nullify는 관계키를 제거하고 disable은 관계를 계속 유지한다.