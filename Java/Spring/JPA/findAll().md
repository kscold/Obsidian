- Spring Data [[JPA(Java Persistence API)]]에서 제공하는 JpaRepository [[인터페이스(Interface)]]에 정의된 메서드 중 하나이다. 
- 이 메서드는 해당 엔티티(테이블)에 저장된 모든 레코드를 조회하는 역할을 합니다.
- findAll() [[메서드(Method)]]는 해당 [[리파지터리(repository)]]를 통해 해당 [[엔티티(entity)]](테이블)에 저장된 모든 [[레코드(Record)]]를 조회하는 역할을 한다.
- 반환값은 [[List]]`<T>` [[객체(Object)]]가 리턴된다.

- 만약 반환하는 값이 [[lterable]]같은 경우에 다운[[캐스팅(casting)]]를 통해 