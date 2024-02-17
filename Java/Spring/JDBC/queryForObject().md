- queryForObject() 메서드는 기본적으로 단건을 조회할 때 사용하는 [[메서드(Method)]]다. 

- 즉, [[SQL]]의 DML 중 [[SELECT]]를 실행했을 때 하나의 [[객체(Object)]] 결과값이 반환된다.

## 문법

```java
queryForObject(쿼리문, 반환할 클래스, ?에 전달할 인자) // 단건 조회
```

- 첫번째 매개변수는 [[PreparedStatement]]를 만들기 위한 [[SQL]] [[쿼리(query)]]문이다.
- 두번째 매개변수는 [[RowMapper]] [[콜백 함수(Callback Fuction)]]을 넣는다.
- 세번째 매개변수는 [[SQL]]의 [[?(Placeholder)]](치환자)에 바인딩할 값을 차례대로 넣으면 된다.

## 예시

- 아래 코드와 같이 결과 [[쿼리(query)]]라인이 1개일 때 사용한다. 

```java
// RoleDaoSqls.java (sql문을 모아놓은 java 클래스 파일)
public static final String SELECT_ID = "SELECT role_id FROM role where description=?";

// RoleDao.java
public List<Integer> selectId(String description){
    return this.jdbcTemplate.queryForList(SELECT_ID, Integer.class, description);
}

// main.java
ApplicationContext ac = new AnnotationConfigApplicationContext(ApplicationConfig.class);

RoleDao roleDao = ac.getBean(RoleDao.class);

Integer roleId = roleDao.selectId("Developer");
// Integer list = roleDao.selectId("CEO"); -> Incorrect result size: expected 1, actual 2 에러

System.out.println(roleId);
```

- 이와 같이 queryForObject 메서드는 단 한 개의 결과만 가져올 수 있기 때문에 주석 처리된  문장은 에러가 발생한다. 
- CEO에 해당하는 결과는 2개 쿼리라인이 발생한다.

- 만약 Integer 같은 기존에 선언된 타입이 아니라 우리가 선언한 [[클래스(Class)]] 타입으로 반환받고 싶다면 [[RowMapper]]를 사용할 수 있다. 

- 이를 위해서 우리는 두 가지 방법을 사용할 수 있다.
- 첫 번째 방법은 아래와 같다.

```java
// RoleDaoSqls.java
public static final String SELECT_ONE_ROW = "SELECT role_id, description FROM role WHERE role_id=?";

// RoleMapper.java
public class RoleMapper implements RowMapper<Role> {
	@Override
	public Role mapRow(ResultSet rs, int rowNum) throws SQLException{
		Role role = new Role();
		
		role.setRoleId(rs.getInt("role_id"));
		role.setDescription(rs.getString("description"));
		
		return role;
	}
}

// RoleDao.java
public Role selectOneRow1(int roleId){
    return this.jdbcTemplate.queryForObject(SELECT_ONE_ROW, new RoleMapper(), roleId);
}

// main문
public static void main(String[] args) {
    ApplicationContext ac = new AnnotationConfigApplicationContext(ApplicationConfig.class);

    RoleDao roleDao = ac.getBean(RoleDao.class);

    Role role = roleDao.selectOneRow1(100);
    System.out.println(role);	
}
```

- 위와 같이 [[RowMapper]] [[인터페이스(Interface)|인터페이스(Interface)]]를 기반으로 클래스를 만들고 해당 클래스에서 mapRow 메서드를 작성해주면 된다. 

- 이때 mapRow 메서드의 rs는 [[ResultSet]] 객체, rowNum은 결과로 받은 sql 행의 개수라고 생각하면 된다. 
- 그리고 만든 클래스의 객체를 생성하여 위와 같이 queryForObject()로 넘겨주면 Role 객체를 얻어올 수 있다.

- 위와 같이 클래스를 만들지 않고 [[Java/필드(Field)|필드(Field)]]로 선언하여 사용할 수도 있다.

```java
// RoleDao.java
private RowMapper<Role> roleMapper = new RowMapper<Role>() {
	@Override
	public Role mapRow(ResultSet rs, int rowNum) throws SQLException{
		Role role = new Role();

		role.setRoleId(rs.getInt("role_id"));
		role.setDescription(rs.getString("description"));
		
		return role;
	}
};

public Role selectOneRow1(int roleId){
    return this.jdbcTemplate.queryForObject(SELECT_ONE_ROW, roleMapper, roleId);
}
```

혹은 아래와 같이 queryForObject를 사용할 때 선언하는 방식도 사용할 수 있다.

```java
// RoleDao.java
public Role selectOneRow2(int roleId){
    return this.jdbcTemplate.queryForObject(SELECT_ONE_ROW, 
            (rs, rowNum) -> new Role(
                    rs.getInt("role_id"),
                    rs.getString("description")
                    ),
            roleId);
}

// main문
public static void main(String[] args) {
	ApplicationContext ac = new AnnotationConfigApplicationContext(ApplicationConfig.class);

    RoleDao roleDao = ac.getBean(RoleDao.class);

    Role role = roleDao.selectOneRow2(100);
    System.out.println(role);	
}
```