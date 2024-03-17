- [[SQL]]에서 사용하는 [[JOIN]]의 종류는 아니고 [[JPQL(Java Persitence Query Language)]]에서 성능 최적화를 위해 제공하는 기능이다.
- 이것은 연관된 [[엔티티(Entity)]]나 컬렉션([[Collection]])을 한 번에 같이 조회하는 기능인데 join fetch 명령어로 사용할 수 있다.

- 예를 들어 fetch Join을 사용해서 회원(Member) [[엔티티(Entity)]]를 조회하면서 연관된 팀(Team) [[엔티티(Entity)]]도 함께 조회하는 [[JPQL(Java Persitence Query Language)]]을 실행한다.