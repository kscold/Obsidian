- 자바와 [[관계형 데이터베이스(Relational DataBase)]]를 연결하기 위해서 connection [[객체(Object)]]를 생성해야 한다.

## 예시

```java
import java.sql.Connection; 
import java.sql.DriverManager; 
import java.sql.SQLException;

public class JDBC1 {
	public static void main(String[] args) {
		String url = "jdbc:mysql://localhost:3306/project?serverTimezone=UTC";
		String id = "root";
		String pw = "1234"; 
		
		Connection conn = null; 
		
		try { 
			Class.forName("com.mysql.cj.jdbc.Driver"); 
			conn = DriverManager.getConnection(url, id, pw); 
			System.out.println("Connection 객체 생성성공"); 
		} catch(ClassNotFoundException e) { 
			System.out.println("드라이버 로드 실패"); 
		} catch(SQLException e) { 
			System.out.println("Connection 객체 생성 실패"); 
		} finally { 
			try { 
				if(conn != null) 
					conn.close(); 
			} catch(SQLException e) { 
				System.out.println("conn.close() 에러"); 
			}
		}
	}
}
```

- 위의 코드는 Class.forName() [[메서드(Method)]]를 통해 생성되는 DriverManager [[클래스(Class)]]를 사용하여 DriverManager.[[getConnection()]]와 같은 메서드를 사용했다.

- 위의 코드를 설명하면 다음과 같다.

	1. String 변수로 url, id, pw를 선언하고 Connection conn(connection 객체)을 전역변수로 선언한다.
	
	2. Class.forName() Class 라는 이름을 가진 클래스는 JVM에서 동작할 클래스들의 정보를 묘사하는 일종의 메타클래스이다. (데이터 베이스와 연결할 드라이버 클래스를 찾아서 로드하는 역할을 한다.)
	
	3. conn = DriverManager.getConnection(url, id, pw); 선언한 conn에 생성된 Connection 객체 대입한다. 
	
	4. System.out.println("Connection"); 객체가 정상적으로 생성되면 이 문장이 나오게 한다.
	
	5. catch (ClassNotFoundException e) 위 2번의 Class.forName에 해당하는 예외처리를 해주고, 여기서 에러가 있다면 드라이버 로드에 실패한 것이므로 에러문장을 "드라이버 로드 실패"라고 써준다.
	
	6. catch (SQLException e) 위 3번의 해당하는 예외처리를 해주고 SQL Server에서 경고 또는 오류를 반환할 때 throw되는 예외로 이 경우, Connection 객체 생성에 실패한 것이므로 "Connection 객체 생성 실패"라고 써준다.
	
	7. finally{ ... } 예외처리 없이 드라이버를 로드하고 객체생성이 정상적으로 완료되었다면 finally 를 이용하여 꼭 실행되게 한다.
	
	8. if(conn != null); 객체가 정상적으로 생성되었다면 더이상 conn은 null 값을 가지지 않으므로 conn.close();로 Connection객체를 닫아준다.
	
	9. catch (SQLException e) 마지막의 예외처리 문장은 어떤 예외로 인해 conn.colse() 실행에 문제가 생겼을 때 throw하는 용도로 써준다.

​