- 개발자가 JDBC 기술을 쉽게 사용할 수 있도록 도와주는 클래스로 아래와 같은 작업을 대신 처리한다.

- Connection 획득
- statement 생성 및 실행
- Connection, statement, resultset 종료
- SQL 조회, 업데이트, ResultSet 반복호출 등
- JDBC 예외 발생시 org.springframework.dao 패키지에 정의된 일반 예외로 변환

위와 같은 기능들을 대신 해주기 때문에 이전에 작성한 JDBC 코드들은 훨씬 간편해진다. 

- 아래는 JDBC Template을 선언한 예시

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

위와 같이 Dao 클래스에는 일반적으로 @Repository를 붙여 Component Scan시 Bean으로 등록하도록 해준다. 클래스 안을 살펴보면 생성자에서 dataSource를 받아 JdbcTemplate 객체 생성 시 넘겨준다. 이와 같이 기본 생성자 없이 인자를 전달받는 생성자만 있다면 Spring은 Bean에 등록된 해당 자료형 객체를 찾아 넣어준다. 즉, dataSource 객체는 Bean으로 등록된 상태여야 한다.

[[queryForObject()]]
[[query()]]
