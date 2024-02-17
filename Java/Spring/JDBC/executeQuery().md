- [[SELECT]] 문과 같은 [[쿼리(query)]]문을 실행할 때 사용한다.

- 쿼리를 실행하고, 결과를 [[ResultSet]] [[객체(Object)]]로 반환한다.
- 반환된 [[ResultSet]] 객체를 통해 결과를 가져올 수 있다.

- [[SELECT]]는 하나 이상의 [[레코드(Record)]]를 조회하게 되는데 이 때 결과 집합을 반환한다.
- ResultSet은 결과 세트에 대한 커서를 포함하므로 사용자가 [[쿼리(query)]] 결과를 반복적으로 가져올 수 있다.

- 커서는 데이터베이스에서 조회된 결과 집합에서 현재 위치를 나태내는 포인터로 이를 이용해 하나의 행([[레코드(Record)]])을 읽어오고, 디음 행([[레코드(Record)]])으로 이동하여 원하는 레코드를 순차적으로 탐색할 수 있다.

## 예시

- 회원의 이메일을 입력해서 해당 회원의 정보를 가져오는 기능을 구현했다.

- [[DAO(Data Access Object)]]에서는 sql을 실행하고 해당 결과를 DB로 반환받는다.

- 이 때 [[SELECT]]문은 여러 [[레코드(Record)]]를 조회하면서 결과 집합을 반환하기 때문에 executeQuery()사용해 실행하고 if(rs.[[next()]]) 를 통해 다음 결과가 있을 경우 커서를 열([[속성(Attribute)]] 혹은 [[Spring/SQL/필드(Field)|필드(Field)]])을 따라 하나하나 이동해가며 member [[객체(Object)]]에 저장하고 반환하게 된다.

```java
import java.io.FileInputStream;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.List;
import java.util.Properties;

import edu.kh.community.model.vo.Member;

import static edu.kh.community.common.JDBCTemplate.*;

public class MemberDAO {
	private Statement stmt;
	private PreparedStatement pstmt;
	private ResultSet rs;
	private Properties prop;
	
	public MemberDAO() {
		try {
			prop = new Properties();
			
			String filePath = MemberDAO.class.getResource("/edu/kh/community/sql/member-sql.xml").getPath();
			
			prop.loadFromXML(new FileInputStream(filePath));
			
		} catch(Exception e) {
			e.printStackTrace();
		}
	}
	
	public Member selectOne(Connection conn, String memberEmail) throws Exception {
	
		Member member = null;
		
		try {
			String sql = 'SELECT MEMBER_EMAIL, MEMBER_NICK , MEMBER_TEL , MEMBER_ADDR,
						  TO_CHAR(ENROLL_DT, 'YYYY"년" MM"월" DD"일"') AS ENROLL_DATE
						  FROM "MEMBER" 
						  WHERE MEMBER_EMAIL = ?
						  AND SECESSION_FL = 'N'';
			
			pstmt = conn.prepareStatement(sql);
			pstmt.setString(1, memberEmail);
			
			rs = pstmt.executeQuery();
			
			if(rs.next()) {
				member = new Member();
				member.setMemberEmail(rs.getString(1));
				member.setMemberNickname(rs.getString(2));
				member.setMemberTel(rs.getString(3));
				member.setMemberAddress(rs.getString(4));
				member.setEnrollDate(rs.getString(5));
			}
			
			
		}finally {
			
			close(rs);
			close(pstmt);
			
			
		}
		
		return member;
	}
```

- 아래 코드는 위의 코드 sql 부분 해석이다.

```sql
SELECT 
	MEMBER_EMAIL, 
	MEMBER_NICK, 
	MEMBER_TEL, 
	MEMBER_ADDR, 
	TO_CHAR(ENROLL_DT, 'YYYY"년" MM"월" DD"일"') AS ENROLL_DATE 
FROM 
	MEMBER
WHERE 
	MEMBER_EMAIL = ? 
	AND SECESSION_FL = 'N'
```

1. SELECT 절은 MEMBER_EMAIL, MEMBER_NICK, MEMBER_TEL, MEMBER_ADDR 및 ENROLL_DT(등록 날짜) 열을 선택한다.

    - ENROLL_DT(등록 날짜)는 [[TO_CHAR]] 함수를 사용하여 'YYYY"년" MM"월" DD"일"' 형식으로 변환된다.
    - AS 키워드를 사용하여 ENROLL_DT(등록 날짜)를 ENROLL_DATE로 별칭 지정한다.
    
2. **FROM 절**: 회원 테이블(MEMBER)에서 데이터를 선택합니다.

3. WHERE 절은 다음의 조건을 만족하는 행을 선택한다.

    - 회원 이메일(MEMBER_EMAIL)이 주어진 이메일과 일치하는지 확인한다.
    - 탈퇴 여부(SECESSION_FL)가 'N'(탈퇴하지 않음)인지 확인한다.

- 이때 SQL 쿼리의 WHERE 절에서 사용된 [[?(Placeholder)]]는 파라미터 바인딩을 나타내며, 이 값은 실행 시점에 구체적인 값으로 대체되어 실행된다.