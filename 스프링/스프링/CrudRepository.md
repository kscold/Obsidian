- CrudRepository는 [[JPA(Java Persistence API)]]에서 제공하는 인터페이스로 이를 상속해 [[엔티티(entity)]]를 관리(생성, 조회, 수정, 삭제)할 수 있다.
- 클래스를 선언 후 extents를 통해 CrudRepository선택하여 가져올 수 있다.


- CrudRepository에 홑화살괄호(<>)를 붙이고 그 안에 다음과 같이 2개의 제네릭 요소를 받는다.
- T: 관리 대상 엔티티의 클래스 타입이다. 책 예시는 Article이다.
- ID: 관리 대상 엔티티의 대푯값 타입니다. Article.java 파일에 가 보면 id를 대푯값으로 지정했다. id의 타입은 Long이므로 Long을 입력한다.