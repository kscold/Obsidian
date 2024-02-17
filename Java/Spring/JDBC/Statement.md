- Statement(java.sql.Statement) [[클래스(Class)]]는 [[Connection]]으로 연결한 객체에게, [[쿼리(query)]] 작업을 실행하기 위한 [[객체(Object)]]이다.
  
 - 정적인 [[쿼리(query)]]를 처리할 수 있다.  
 - 즉 [[쿼리(query)]]를 처리할 때는 반드시 값이 미리 입력되어야만 처리가 가능하다.

- 향상된 버전으로 [[PreparedStatement]] [[클래스(Class)]]가 있다.

## 예시

- 밑의 코드는 Statement를 생성하고 사용하는 방법이다.
- Statement [[객체(Object)]]를 생성하기 위해서는 [[Connection]]이 먼저 연결되야 한다.

```java
package jdbc;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class DBConnection {

	private Connection con; // Connection 객체 정의
	private Statement stmt;
	
	public DBConnection() {
		try {
			String url = "jdbc:mysql://localhost/?characterEncoding=UTF-8&serverTimezone=UTC";
			String user = "user";
			String passwd = "1234";
			
			con = DriverManager.getConnection(url, user, passwd); // DB 정보 입력
			System.out.println("DB연결 성공");
			
			stmt = con.createStatement();
			System.out.println("Statement객체 생성 성공");
			
			stmt.close();
			con.close();

		} catch (SQLException e) {
			System.out.println("DB연결 실패");
			System.out.print("사유 : " + e.getMessage());
		}
	}

	public static void main(String[] args) {
		new DBConnection();
	}

}
```

  
- [[Connection]]을 연결했으면 `Statement stmt = con.createStatement();`문을 통해 Statement 객체를 생성할 수 있다. 
  
## [[쿼리(query)]]문을 처리하는 방법

- executeUpdate()와 executeQuery() [[메서드(Method)]]가 있다.

- 각각의 메서드는 조금씩 다른 역할을 수행한다.

### [[executeUpdate()]]

- 조회문([[Servlet]], [[SHOW]] 등)을 제외한 [[CREATE]], [[DROP]], [[INSERT]], [[DELETE]], [[UPDATE]] 등등 문을 처리할 때 사용한다.

### [[executeQuery()]]

- 조회문([[SELECT]], [[SHOW]] 등)을 실행할 목적으로 사용한다.
