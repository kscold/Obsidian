- query 메서드를 사용하면 queryForObject와 다르게 여러 건의 결과를 받아올 수 있다. 

- 이때 "여러 건"의 결과를 받아오므로 해당 메서드의 결과는 [[List]] 자료형에 담아진다. 


## 예시

```java
// RoleDaoSqls.java
public static final String SELECT_ALL = "SELECT role_id, description FROM role order by role_id";

// RoleDao.java
public List<Role> selectAll(){
    return this.jdbcTemplate.query(SELECT_ALL, 
            (rs, rowNum) -> new Role(
                    rs.getInt("role_id"),
                    rs.getString("description")));
}

// main문
public static void main(String[] args) {
    ApplicationContext ac = new AnnotationConfigApplicationContext(ApplicationConfig.class);

    RoleDao roleDao = ac.getBean(RoleDao.class);

    List<Role> roles = roleDao.selectAll();

    for(Role role: roles) {
        System.out.println(role);
    }
}
```