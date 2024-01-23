- Create Read Update Delete의 약자로 [[HTTP]] [[메서드(Method)]]에서 기본이 되는 동작이다.

- [[JPA(Java Persistence API)]]에서는 [[CrudRepository]]를 사용한다.

- 이러한 CRUD는 [[SQL]] 문으로 적용할 수 있는데, 이 개념이 [[HTTP]]의 [[메서드(Method)]]에도 그대로 적용된다.


| 데이터 관리 | SQL | HTTP |
| ---- | ---- | ---- |
| 데이터 생성(Create) | [[INSERT]] | POST |
| 데이터 조회(Read) | [[Spring/sql/SELECT\|SELECT]] | GET |
| 데이터 수정(Update) | [[UPDATE]] | PATCH(PUT) |
| 데이터 삭제(Delete) | [[DELETE]] | DELETE |

