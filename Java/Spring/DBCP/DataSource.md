- 커넥션 풀([[DBCP(Database Connection Pool)]])에는 여러 개의 [[Connection]] [[객체(Object)]]가 생성되어 운용되는데, 이를 직접 웹 애플리케이션에서 다루기 힘들기 때문에 DataSource라는 개념을 도입하여 사용한다.

- 커넥션 풀의 Connection을 관리하기 위한 [[객체(Object)]]이다.
- JNDI Server를 통해서 이용된다.
- DataSource 객체를 통해서 필요한 [[Connection]]을 획득, 반납 등의 작업을 한다.

## DataSource의 사용법

1. JNDI Server에서 lookup() [[메서드(Method)]]를 통해 DataSource [[객체(Object)]]를 획득한다.
2. DataSource 객체의 [[getConnection()]] [[메서드(Method)]]를 통해서 Connection Pool([[DBCP(Database Connection Pool)]])에서 Free 상태의 [[Connection]] [[객체(Object)]]를 획득한다.
3. Connection 객체를 통한 DBMS 작업을 수행한다.
4. 모든 작업이 끝나면 DataSource 객체를 통해서 Connection Pool에 [[Connection]]을 반납한다.