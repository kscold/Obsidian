- 데이터를 저장 하려면, DB에 넘겨야 하는데 Java와 sql로 사용하는 언어의 차이 때문에 직접 데이터를 넘기기가 쉽지 않다.
- 따라서 이를 위한 라이브러리가 JPA(Java Persistence API)라고 부르며, 정리하면 서버와 DB 간의 소통에 관여하는 기술이다.

- 자바 표준 ORM(Object-Relational Mapping) 기술이다.
- ORM은 객체 지향 프로그래밍 언어와 [[관계형 데이터베이스(Relational DataBase)]] 사이의 데이터를 매핑하는 기술을 의미한다.

- JPA의 핵심 도구는 [[엔티티(Entity)]]와 [[리파지터리(Repository)]]이 있다.

- 자바 언어로 DB에 명령을 내리는 도구로, 데이터를 객체 지향적으로 관리할 수 있게 해준다.

- JDBC(Java DataBase Connectivity)와 [[JPA(Java Persistence API)]] 모두 바에서 데이터베이스와 상호작용하기 위한 기술이나, JPA는 개발자는 [[SQL]] 쿼리를 직접 작성하는 대신 JPA의 기능을 사용하여 데이터베이스 작업을 수행할 수 있다.