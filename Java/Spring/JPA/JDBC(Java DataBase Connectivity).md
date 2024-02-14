- 자바에서 데이터베이스에 접근할 수 있도록 지원하는 자바 API이다.
- RDB의 쿼리문을 수행하기 위해 자바로 작성된 클래스와 인터페이스이다.
- - 독립적인 인터페이스를 통해 다양한 데이터베이스에 접근하는 코드를 구현할 수 있도록 제공하는 자바 클래스의 표준이다.

![[Pasted image 20240211213841.png]]

## JDBC와 JDBC Template의 차이
### jdbc 사용

```java
private Connection getConnection() {
	return DataSourceUtils.getConnection(dataSource);
    }
```

- getConnection(datasource) 메서드를 사용하여 연결한다.
- 주어진 데이터 소스에서 JDBC 연결한다.

```java
public Member save(Member member) {
	String sql = "insert into member(name) values(?)"; // sql문 실행
	Connection conn = null;
	PreparedStatement pstmt = null;
    ResultSet rs = null;
    
    try {
        conn = getConnection();
        pstmt = conn.prepareStatement(sql,
	        Statement.RETURN_GENERATED_KEYS);
        
        pstmt.setString(1, member.getName());
        pstmt.executeUpdate();
        
        rs = pstmt.getGeneratedKeys();
        
        if (rs.next()) {
            member.setId(rs.getLong(1));
        } else {
            throw new SQLException("id 조회 실패");
        }
        
        return member;
    
    } catch (Exception e) {
        throw new IllegalStateException(e);
    } finally { // 무조건 실행
        close(conn, pstmt, rs); 
    }
}
```

- PreparedStatement는 conn에 접속된 데이터베이스를 통해 sql문과 매개변수를 사용해 데이터 처리를 할 수 있다.

### [[JDBC Template]]

Spring Framework에서 JDBC 프로그래밍을 위해 JdbcTemplate 클래스를 제공하여 JdbcTemplate 클래스를 사용해 손쉽게 DB와 연동할 수 있도록 구현되어 있다.

### **4.  jdbcTemplate 사용**

**- 연결**

```
private final JdbcTemplate jdbcTemplate;

@Autowired
public JdbcTemplateMemberRepository(DataSource dataSource) {
  jdbcTemplate = new JdbcTemplate(dataSource);
}
```

생성자를 통해 의존성만 주입해주면 된다.

**- 사용**

```
@Override
public Member save(Member member) {

  SimpleJdbcInsert jdbcInsert = new SimpleJdbcInsert(jdbcTemplate);
  
  jdbcInsert.withTableName("member").usingGeneratedKeyColumns("id");
  
  Map<String, Object> parameters = new HashMap<>();
  
  parameters.put("name", member.getName());
  
  Number key = jdbcInsert.executeAndReturnKey(new
  	MapSqlParameterSource(parameters));
    
  member.setId(key.longValue());
  
  return member;
}
```

JDBCTemplates는 내부에서 지원해주는 메서드를 통해서 데이터를 처리하기 때문에 좀 더 쉽고 간결하게 코드를 작성 할 수 있다.