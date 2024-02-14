- [[JPA(Java Persistence API)]]는 DB와의 소통을, 보다 쉽게 하고, 그 핵심이 되는 [[인터페이스(Interface)]]를 리파지터리(repository)라 한다.

- [[엔티티(entity)]]가 DB 속 [[테이블(Table)]]에 저장 및 관리될 수 있게 하는 인터페이스이다.
- [[@Repository]] [[어노테이션(Annotation)]]으로 리파지터리를 정의한다.

![[Pasted image 20240106022633.png]]

## [[리파지터리(repository)]]의 [[인터페이스(Interface)]] 구조

- 계층도의 밑으로 내려갈수록 low level 모듈이며 기능 구현이 많다.
### Repository 

- 최상위 [[리파지터리(repository)]] [[인터페이스(Interface)]]이다.

### [[CrudRepository]] 및 ListCrudRepository

- [[엔티티(entity)]]의 [[CRUD]] 기능을 제공한다.
### PagingAndSortingRepository 및 ListPagingAndSortingRepository

- [[엔티티(entity)]]의 페이징 및 sorting 관련 기능들 제공한다.
### [[JpaRepository]]

- [[JPA(Java Persistence API)]] 관련 특화된 기능을 추가로 제공한다. 
- flushing, 배치성 작업 등을 제공한다.
- 추가적으로 CrudRepository와 PagingAndSortingRepository의 기능들을 제공한다.

- 단순하게 CRUD 작업만 한다면 [[CrudRepository]]를, 그 외에 페이징, sorting, jpa 특화 기능들을 사용하길 원한다면 [[JpaRepository]]를 사용하면 된다.

![[Pasted image 20240215001140.png]]