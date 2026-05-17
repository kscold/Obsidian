- FetchType이란, [[JPA(Java Persistence API)]]가 하나의 [[엔티티(Entity)]]를 조회할 때, [[연관 관계(Relationships)]]에 있는 [[객체(Object)]]들을 어떻게 가져올 것이냐를 나타내는 설정값이다.

- [[JPA(Java Persistence API)]]는 ORM기술로, 사용자가 직접 [[쿼리(query)]]를 생성하지 않고, [[JPA(Java Persistence API)]]에서 [[JPQL(Java Persitence Query Language)]]을 이용하여 [[쿼리(query)]]문을 생성하기 때문에 [[객체(Object)]]와 [[Spring/SQL/필드(Field)|필드(Field)]]를 보고 [[쿼리(query)]]를 생성한다.

- 따라서 다른 [[객체(Object)]]와 [[연관 관계(Relationships)]] 매핑이 되어있으면 그 [[객체(Object)]]들까지 조회하게 되는데, 이때 이 [[객체(Object)]]를 어떻게 불러올 것인가를 설정할 수 있다.

- [[즉시 로딩(Eager Loading)]]과 [[지연 로딩(Lazy Loading)]]으로 나눌 수 있다.

## 즉시 로딩(Eager Loading)과 지연 로딩(Lazy Loading)의 장단점 분석

### [[즉시 로딩(Eager Loading)]]

#### 장점

- 연관된 엔티티를 모두 가져올 수 있다.
#### 단점

- [[엔티티(Entity)]]간의 관계가 복잡해질수록 조인([[JOIN]])으로 인한 성능 저하가 나타날 수 있다.
- 불필요한 조인까지 포함해서 처리하는 경우가 많다.

- 예를 들어 Order 연관된 객체가 Member가 N개라면, Order 1개 조회 시 필요하지 않은 Member 객체를 조회하는 쿼리 N개가 생성된다.
- [[JPQL(Java Persitence Query Language)]]에서 N+1 문제를 일으킨다.

### [[지연 로딩(Lazy Loading)]]

#### 장점

- 다른 접근 방식보다 훨씬 적은 초기의 로딩 시간이 걸린다.
- 다른 접근 방식에 비해 메모리 소비량 감소한다.
#### 단점

- 초기화가 지연되면 원하지 않는 순간 성능에 영향을 줄 수 있다.

### 결론

- 따라서 모든 [[연관 관계(Relationships)]]는 [[지연 로딩(Lazy Loading)]]으로 설정한다.
- 즉시 로딩(Eager)는 예측이 어렵고, 어떤 [[SQL]]이 실행될지 추적하기 어렵다. 
- 특히 [[JPQL(Java Persitence Query Language)]]을 실행할 때 N+1 문제가 자주 발생한다.

- 실무에서 모든 연관관계는 [[지연 로딩(Lazy Loading)]](LAZY)으로 설정해야 한다.
- 연관된 엔티티를 함께 [[데이터베이스(DataBase)]]에서 조회해야 하면, fetch join 또는 엔티티 그래프 기능을 사용한다.
- @XToOne(OneToOne, ManyToOne) 관계는 기본이 [[즉시 로딩(Eager Loading)]]이므로 직접 [[지연 로딩(Lazy Loading)]]으로 설정해야 한다.