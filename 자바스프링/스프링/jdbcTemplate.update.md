
- Spring JDBC 프레임워크에서 제공하는 간편한 메서드 중 하나로, SQL UPDATE, INSERT, DELETE 쿼리를 실행하기 위해 사용된다. 이 메서드를 사용하면 JDBC 코드를 간결하게 작성할 수 있습니다.

### 형식

```java
int update(String sql, Object... args)
```

- `sql`: 실행할 SQL 쿼리 문자열입니다.
- `args`: SQL 쿼리에 전달될 매개변수들입니다.

메서드가 반환하는 `int` 값은 실행된 SQL 쿼리에 의해 영향을 받은 행의 수입니다. 이 값은 보통 INSERT, UPDATE, DELETE 쿼리의 경우에 사용됩니다.

예를 들어, 여러분이 제공한 코드에서의 사용 예시는 다음과 같습니다:


```sql
String sql = "UPDATE tbl_admin_member SET " +
"a_m_name = ?, " +     
"a_m_gender = ?, " +     
"a_m_part = ?, " +     
"a_m_position = ?, " +     
"a_m_mail = ?, " +     
"a_m_phone = ?, " +     
"a_m_mod_date = NOW() " +     
"WHERE a_m_no = ?";  

int result = jdbcTemplate.update(sql, adminMemberVo.getA_m_name(),         adminMemberVo.getA_m_gender(), adminMemberVo.getA_m_part(),         adminMemberVo.getA_m_position(), adminMemberVo.getA_m_mail(),         adminMemberVo.getA_m_phone(), adminMemberVo.getA_m_no());
```

이 코드에서 `jdbcTemplate.update` 메서드는 주어진 SQL 쿼리를 실행하고, 해당되는 매개변수들을 전달하여 업데이트된 행의 수를 반환합니다. 반환된 값은 `result` 변수에 저장되어 나중에 확인하거나 처리할 수 있습니다.

Spring의 `JdbcTemplate`는 데이터베이스 작업을 위한 여러 메서드들을 제공하며, 간단하게 SQL 쿼리를 실행하고 결과를 처리하는 데 사용됩니다.