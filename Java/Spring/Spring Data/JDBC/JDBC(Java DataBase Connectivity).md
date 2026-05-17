- 자바에서 데이터베이스에 접근할 수 있도록 지원하는 자바 API이다.

- [[관계형 데이터베이스(Relational DataBase)]](RDB)의 [[쿼리(query)]]문을 수행하기 위해 자바로 작성된 [[클래스(Class)]]와 [[인터페이스(Interface)]]이다.

- 독립적인 인터페이스를 통해 다양한 데이터베이스에 접근하는 코드를 구현할 수 있도록 제공하는 자바 클래스의 표준이다.

- JDBC(Java DataBase Connectivity)와 [[JPA(Java Persistence API)]] 모두 바에서 데이터베이스와 상호작용하기 위한 기술이나, JDBC는 데이터베이스에 대한 접근을 저수준으로 제공하기 때문에 개발자가 직접 SQL 쿼리를 작성하고 관리해야 한ㄷ.

![[Pasted image 20240211213841.png]]

## JDBC 과정

![](https://blog.kakaocdn.net/dn/cd4Rkh/btrO91pb6gh/BwOqdEz4gXwyj8a0iT7dIK/img.png)

- JDBC 연결은 드라이버를 로드하고 연결하여 객체를 받아와야 하는 과정을 가지고 있다.

- 이 과정은 매번 사용자가 요청할 때마다 드라이버를 로드하고 커넥션 객체를 생성하여 연결하고 종료하는 과정이 불편하고 속도와 자원 소모에 대한 단점이 있다.
- 그래서 이 단점을 보완하기 위해 [[DBCP(Database Connection Pool)]]을 사용한다.



## JDBC 관련클래스

![](https://blog.kakaocdn.net/dn/cyvIDQ/btqv95xv2XK/vDMXlVMlrRjQmnXJ18xXV0/img.png)

: java.sql 패키지

### DriverManager

- JVM에서 JDBC 전체를 관리하는 [[클래스(Class)]]이다.
- Driver등록, Connection 연결작업 등이 있다.
### Driver  

- DB를 만드는 Vendor(Oracle, MS-SQL, MYSQL등)을 implements하여 자신들의 DB를 연결할 수 있는 class를 만드는 [[인터페이스(Interface)]]이다.
## [[Connection]]

-  DB와 연결성을 갖는 인터페이스이다.

### [[Statement]]

- [[SQL]]문을 실행하는 인터페이스이다.

## [[ResultSet]]

- 조회된 결과 데이터를 갖는 인터페이스이다.

## JDBC 프로그래밍

1. 제품군 선택 (Oracle, Mysql, MS-SQL 등)
2. 연결객체 생성 (Connection)  
    [**필요요소**]

- DB서버의 주소
- 포트번호(0~63353개.한피시 안에서 서비스 종류를 판별하기 위해 사용)
- 연결계정

3. 실행객체 생성 (Statement)  
    [**관련요소**]

- SQL문 작성
- SQL문 실행요청

4. 결과객체 생성 (ResultSet)

- 행단위 데이터얻기
- 열 데이터 뽑기

---

### 1. 드라이브 로딩 Driver loading (DB제품군 선택)

```
Class.forName("oracle.jdbc.driver.OracleDriver");
// Class.forName("클래스명");
// Class.forName("패키지명.드라이버클래스명");
```

- ClassNotFoundException 오류발생. try-catch문 혹은 예외처리

### 2. 연결객체 생성 Connection (특정 DB서버연결)

```
    Connection conn = DriverManager.getConnection(url, user, password);

    String url = "jdbc:oracle:thin:@localhost:1521:xe"; // "localhost" == "127.0.0.1 == 내 pc
    String user = "scott";
    String password = "tiger";
    //conn = DriverManager.getConnection("jdbc:oracle:thin:@localhost:1521:xe","scott","tiger");

    System.out.println("DB연결 성공");
```

- **url** :접속DB ip, port번호, sid
- **user** : 접속계정
- **pwd** : 접속계정에 대한 비밀번호

### 3. 실행객체 생성 Statement

- SQL문 (DML, DQL 실행가능)

```
    Statement stmt = conn.createStatement();

    //실행요청
    int t = stmt.executeUpdate(sql); //DML 실행
    int t = stmt.executeQuery(sql);  //DQL 실행
```

- DML 작업

```
    // 테이블 dept_copy에서 10번부서를 삭제
        String sql = "DELETE FROM dept_copy WHERE deptno=10";

    // --SQL문 실행요청
        int t = stmt.executeUpdate(sql); // 삭제시점
        System.out.println("T=" + t); //t: 수정 또는 삭제된 행의 갯수
        if (t > 0) {
            System.out.println("삭제성공");// t==> 삭제(또는 수정)된 행의 갯수
        }

        --테이블 dept_copy에서 20번, 30번 부서를 삭제하시오(삭제성공메세지 출력)
        sql = "DELETE FROM dept_copy WHERE deptno IN(20,30)";
        t = stmt.executeUpdate(sql);
        System.out.println("T=" + t);

        if (t > 0) {
            System.out.println("삭제성공");
        } else {
            System.out.println("삭제실패");
        }
```

### 4. 결과객체 생성

- ResultSet : 조회된 결과(행열데이터)를 저장
    
- DQL 작업
    
    조회요청, 조회된 결과값(행열데이터) 리턴
    
    ```
      ResultSet 변수명 = Statement명.executeQuery(sql); //리턴값 ResultSet
      //1. 행단위 데이터 얻기
      rs.next(); //boolean값 리턴. 행 반환시 true 
      if(rs.next()){} // 0번 또는 1번 출력시. WHERE절에 primary key 비교
      while(rs.next()){} //조회된 행의 갯수가 2행이상이 예상되어질 때
    ```
    
    얻어온 행 안에서 개별 열데이터 얻기
    
    ```
      rs.get자료형(columnIndex); // 칼럼번호
      rs.get자료형(columnLabel); // 이름, 별명
    
      // 20번 부서의 부서번호, 부서명, 부서위치를 출력하시오
      sql = "SELECT deptno, dname, loc FROM dept WHERE deptno=20";
    
      ResultSet rs = stmt.executeQuery(sql);
      rs.next();
      int deptno = rs.getInt(1); // 첫번째 칼럼데이터
      String dname = rs.getString(2);// 두번째 칼럼데이터
      String loc = rs.getString(3);// 세번째 칼럼데이터
      System.out.println(deptno + dname + loc);
      System.out.println("=======");
    
      sql = "SELECT deptno, dname name, loc FROM dept";
      rs = stmt.executeQuery(sql);
      while(rs.next()) { //rs.next가 while 소괄호 안에서 실행됨
      //rs.getInt(1)    rs.getString(2) 숫자 표기가능
          deptno = rs.getInt("deptno"); 
          dname = rs.getString("name");
          loc = rs.getString(3);
          System.out.println(deptno +"\t"+ dname +"\t"+ loc);
      } 
    ```
    

---

### AutoCommit 설정

```
    conn.setAutoCommit(false); //기본값 true
    conn.commit();
    conn.rollback();
```

### conn.close()

:연결끊는 객체

- DB는 데이터 공유를 위해 사용
- Connection은 유한개⇒ 다른사람을 위해 사용한 연결객체는 반환
- DB자원: Connection 생성→ Statement 생성 → ResultSet생성
- DB자원 반환은 역순으로 해주면 된다.

```
    if(rs != null) rs.close();   
    if(stmt != null) stmt.close();   
    if(conn != null) conn.close();

    // if문 사용안할시 NullPoineException 오류발생
```

- Connection만 끊어주면 Statement와 ResultSet도 자동적으로 끊김


## JDBC와 JDBC Template의 차이
### JDBC 사용

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

- Spring Framework에서 JDBC 프로그래밍을 위해 JdbcTemplate 클래스를 제공하여 JdbcTemplate 클래스를 사용해 손쉽게 DB와 연동할 수 있도록 구현되어 있다.

###  jdbcTemplate 사용

``` java
// 연결
private final JdbcTemplate jdbcTemplate;

@Autowired
public JdbcTemplateMemberRepository(DataSource dataSource) {
	jdbcTemplate = new JdbcTemplate(dataSource);
}
```

- [[생성자(constructor)]]를 통해 의존성만 주입([[DI(Dependency Injection)]])을 해주면 된다.

```java
// 사용

@Override
public Member save(Member member) {
	SimpleJdbcInsert jdbcInsert = new SimpleJdbcInsert(jdbcTemplate);
	jdbcInsert.withTableName("member").usingGeneratedKeyColumns("id");
	
	Map<String, Object> parameters = new HashMap<>();
	
	parameters.put("name", member.getName());
	
	Number key = jdbcInsert.executeAndReturnKey(new MapSqlParameterSource(parameters));
    
    member.setId(key.longValue());
    
    return member;
}
```

- JDBCTemplates는 내부에서 지원해주는 메서드를 통해서 데이터를 처리하기 때문에 좀 더 쉽고 간결하게 코드를 작성 할 수 있다.