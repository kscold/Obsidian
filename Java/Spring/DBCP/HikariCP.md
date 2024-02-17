-  빠르고, 간단하고 믿을 수 있는 HikariCP는 "오버헤드(어떤 처리를 하기 위해 들어가는 간접적인 처리 시간 · 메모리 등을 의미한다.) 제로"의 프로덕션 지원 [[JDBC(Java DataBase Connectivity)]] 연결 풀입니다. 

- 대략 130KB이며 이 라이브러리는 매우 가볍다.

- HiKariCP는 데이터베이스 연결(Connection)을 관리해 주는 도구(라이브러리)이다.
- HiKariCP에서 커넥션 풀(Connection Pool) [[DBCP(Database Connection Pool)]]이 설정된 커넥션의 사이즈만큼의 연결을 허용하며 [[HTTP]] 요청에 대해 순차적으로 DB 커넥션을 처리해 주는 기능을 수행한다. 

- HiKariCP는 DBCP(Database Connection Pool)이며 HikariCP, Common DBCP 등 라이브러리가 존재하는데 가볍고 빠르게 처리할 수 있다는 장점이 있는 HikariCP를 사용한다.

