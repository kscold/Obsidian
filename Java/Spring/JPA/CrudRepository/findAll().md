- Spring Data [[JPA(Java Persistence API)]]에서 제공하는 JpaRepository [[인터페이스(Interface)]]에 정의된 메서드 중 하나이다. 
- - findAll() [[메서드(Method)]]는 해당 [[리파지터리(repository)]]를 통해 해당 [[인스턴스(Instance)]]([[엔티티(entity)]] 혹은 [[테이블(Table)]])에 저장된 모든 [[레코드(Record)]]를 조회하는 역할을 한다.

- 결괏값은 [[Iterable]]`<T>`타입으로 반환한다.
- 만약 반환하는 값이 [[Iterable]]같은 경우에 다운[[형변환(Casting)]]를 통해 [[List]]`<T>`로 바꿀 수 있다.