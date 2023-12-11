- ResultSet(java.sql.ResultSet)은 executeQuery(String sql)을 통해 쿼리 실행하면 ResultSet타입으로 반환을 해주어 결과값을 저장할 수 있다.

### ResultSet의 사용이유
- 결과값을 저장할 수 있다.

- 저장된 값을 한 행 단위로 불러올 수 있다.

- 한 행에서 값을 가져올 때는 타입을 지정해 불러올 수 있다.

```java
package jdbc; 

import java.sql.Connection; 
import java.sql.DriverManager; 
import java.sql.ResultSet; 
import java.sql.SQLException; 
import java.sql.Statement; 

public class OJDBC { 
	private Connection con; 
	private Statement stmt; 
	private ResultSet rs; 
	
	public OJDBC() { 
		try { 
			String OracleUrl = "jdbc:oracle:thin:@localhost:1521:XE"; 
			String OracleUser = "system"; 
			String OraclePasswd = "1234"; 
			
			con = DriverManager.getConnection(OracleUrl, OracleUser, OraclePasswd); 
			
			System.out.println("DB연결 성공"); 
			stmt = con.createStatement(); 
			System.out.println("Statement객체 생성 성공"); 
			rs = stmt.executeQuery("select * from coffee"); //조회한 결과들을 ResultSet에 rs에 저장한다. 
			rs.close(); stmt.close(); 
			con.close(); 
		} catch (SQLException e) {
			System.out.println("DB연결 실패하거나, SQL문이 틀렸습니다."); 
			System.out.print("사유 : " + e.getMessage()); 
		} 
	} 
	
	public static void main(String[] args) { 
	// TODO Auto-generated method stub 
		new OJDBC(); 
	} 
}
```


**ResultSet**은 **Statement**를 통해 값을 저장할 수 있다.

이때, 사용하는 메소드는 executeQuery(String sql) 메소드를 통해 저장할 수 있다.

next() 메소드를 통해, 선택되는 행을 바꿀 수 있다.

그리고, 다음행이 내려갈 다음행이 있을 경우 **TRUE**를 반환하고, 없을 경우 **FALSE**를 반환한다.

![](https://blog.kakaocdn.net/dn/k9YWa/btqEZ9kuQ06/AvAkgE4XORM0mQIi2M5hx0/img.png)

커서가 현재 아무행도 가르키지 않고 있다.

위 이미지에서 커서가 현재 아무것도 가리키지 않아, 만약 getString()/ getInt()등 메소드를 사용했을 경우 에러가 발생한다.

![](https://blog.kakaocdn.net/dn/cadHMb/btqE00fBm5t/yKIF92P6EdtKSFir3QsZ90/img.png)

커서가 한칸이 내려갔다

그래서 **next()** 메소드를 실행하면 위와 같이 **커서**가 잡하게 된다.


**get타입()** 메소드를 통해 데이터를 불러올 수 있다.

![](https://blog.kakaocdn.net/dn/vluhM/btqE0xyq3Lo/thGr8TrNnZoKj0RxQMEHN1/img.png)

많은 타입을 지정할 수 있다.

**get타입**은 컬럼의 숫자나, 컬럼의 이름을 지정해서 값을 불러 올 수 있게 된다.

예시) **c_no**의 INT형 컬럼이 있다. 이 컬럼의 번호는 **1**번째 이다. 그러면 **getInt(1)** 이나 / getInt("c_no")를 통해 불러올 수 있다.

```java
package jdbc;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class OJDBC {

	private Connection con;
	private Statement stmt;
	private ResultSet rs;
	
	public OJDBC() {
		try {
			String OracleUrl = "jdbc:oracle:thin:@localhost:1521:XE";
			String OracleUser = "system";
			String OraclePasswd = "1234";

			con = DriverManager.getConnection(OracleUrl, OracleUser, OraclePasswd);
			System.out.println("DB연결 성공");

			stmt = con.createStatement();
			System.out.println("Statement객체 생성 성공");
			
			rs = stmt.executeQuery("select * from coffee"); //조회한 결과들을 ResultSet에 rs에 저장한다.
			
			while(rs.next()) { //rs.next()를 통해 다음행을 내려갈 수 있으면 true를 반환하고, 커서를 한칸 내린다. 다음행이 없으면 false를 반환한다.
				System.out.println(rs.getInt(1) + "\t" + rs.getString(2)); //getInt(1)은 컬럼의 1번째 값을 Int형으로 가져온다. / getString(2)는 컬럼의 2번째 값을 String형으로 가져온다.
			}
			
			rs.close();
			stmt.close();
			con.close();

		} catch (SQLException e) {
			System.out.println("DB연결 실패하거나, SQL문이 틀렸습니다.");
			System.out.print("사유 : " + e.getMessage());
		}
	}

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		new OJDBC();
	}

}
```

위 코드처럼 작성을 할경우 rs.next()의 결과값이 TRUE와 FALSE로 반환되기 때문에, 다음으로 넘어갈 행이 없으면 while문을 나오게 된다.

![](https://blog.kakaocdn.net/dn/kJijQ/btqE0xFcuuF/5RPHAE0WKVJnLhrFdfkuN1/img.png)

결과값들이 모두 출력됬다.

이렇게 하면 모든 조회된 결과값이 출력된것을 확인할 수 있다.
