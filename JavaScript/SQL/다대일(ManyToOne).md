- [[객체(Object)]]는 양방향 참조가 존재하기 때문에 어느 쪽에서 [[외래 키(Foreign Key)]]를 관리할지 정해야 한다.

- [[외래 키(Foreign Key)]]를 가진 [[테이블(Table)]]을 매핑한 [[엔티티(Entity)]]에서 [[외래 키(Foreign Key)]]를 관리하는게 효율적이다.

- [[일대다(OneToMany)]], 다대일 관계에서 항상 '다'쪽이 [[외래 키(Foreign Key)]]를 가진다.
- 1:N([[일대다(OneToMany)]]), N:1([[다대일(ManyToOne)]]) 경우에 항상 N에 FK([[외래 키(Foreign Key)]])가 있으므로 N 쪽이 연관관계의 주인이다.
- 즉, [[외래 키(Foreign Key)]]를 가진 [[엔티티(Entity)]]가 주인이라고 생각하면 쉽다.

- 주인이 아닌 쪽은 [[외래 키(Foreign Key)]]를 변경할 수 없고 읽기만 가능하다.

- 양방향([[다대다(ManyToMany)]])은 FK([[외래 키(Foreign Key)]])가 있는 쪽이 [[연관 관계(Relationships)]]의 주인이다.


- FK([[외래 키(Foreign Key)]])를 관리할 때 [[연관 관계(Relationships)]]의 주인만 사용한다.

- 양방향 [[연관 관계(Relationships)]]는 항상 서로를 참조해야 한다.