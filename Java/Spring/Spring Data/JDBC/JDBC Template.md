- 개발자가 [[JDBC(Java DataBase Connectivity)]] 기술을 쉽게 사용할 수 있도록 도와주는 [[클래스(Class)]]로 아래와 같은 작업을 대신 처리한다.

## 예시

- 아래는 JDBC Template을 선언한 예시이다.
	- Connection 획득한다.
	- statement 생성 및 실행한다.
	- Connection, statement, resultset 종료한다.
	- SQL 조회, 업데이트, ResultSet 반복호출 등이 가능하다.
	- JDBC 예외 발생시 org.springframework.dao 패키지에 정의된 일반 예외로 변환한다.

- 위와 같은 기능들을 대신 해주기 때문에 작성한 바닐라 [[JDBC(Java DataBase Connectivity)]] 코드들은 훨씬 간편해진다. 

```java
@Repository
public class RoleDao {
    private JdbcTemplate jdbcTemplate;
	
    @Autowired
    public RoleDao(DataSource dataSource) {
        this.jdbcTemplate = new JdbcTemplate(dataSource);
    }
    ...
}
```

- 위와 같이 Dao 클래스에는 일반적으로 [[@Repository]]를 붙여 [[Component Scan]]시 [[Bean]]으로 등록하도록 해준다.
- 클래스 안을 살펴보면 생성자에서 dataSource를 받아 Jdbc Template [[객체(Object)]] 생성 시 넘겨준다.

- 이와 같이 기본 생성자 없이 인자를 전달받는 생성자만 있다면 Spring은 [[Bean]]에 등록된 해당 자료형 객체를 찾아 넣어준다.
- 즉, dataSource [[객체(Object)]]는 [[Bean]]으로 등록된 상태여야 한다.

## JDBC Template 메서드

- [[queryForObject()]]
- [[query()]]
