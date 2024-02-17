- ResultSet은 [[executeQuery()]]을 통해 쿼리 실행하면 ResultSet타입으로 반환을 해주어 결과값을 저장할 수 있다.

- java.sql.ResultSet [[인터페이스(Interface)]]에 정의되어 있다.

- ResultSet은 [[SQL]] [[쿼리(query)]]에 의해 생성된 [[테이블(Table)]]의 값을 가지게 된다.

- 즉, ResultSet은 결과값을 저장할 수 있다.
- 저장된 값을 한 행([[레코드(Record)]]) 단위로 불러올 수 있다.
- 한 행에서 값을 가져올 때는 [[타입(Type)]]을 지정해 불러올 수 있다.

## Cursor

- ResultSet [[객체(Object)]]는 '커서(Cursor)' 라고 불리는 것을 가지고 있는데, 그것으로 ResultSet에서 특정 행([[레코드(Record)]])에 대한 참조를 조작할 수 있다.
- 커서는 초기에 첫 번째 행의 이전에 위치하도록 되어있다.

- ResultSet 객체의 [[next()]] [[메서드(Method)]]드를 사용하면 다음 위치로 커서를 옮길 수 있다.

### Cursor의 메서드

#### first()

- 커서를 첫 번째 행으로 옮긴다.
#### last() 

- 커서를 마지막 행으로 옮긴다.
#### beforeFirst()

- 커서를 첫번째 행 이전으로 옮긴다.
#### afterLast()

- 커서를 마지막 행 다음으로 옮긴다.
#### next()

- 커서를 다음 행으로 옮기고, 다음행이 있으면 true, 없으면 false를 리턴한다.
#### previous()

- 커서를 이전 행으로 옮긴다.

### Cursor 예시

- ResultSet에서 행을 처리하는데 반복문을 사용하며 [[next()]] [[메서드(Method)]]가 유효한 행([[레코드(Record)]])이 있으면 true, 없으면 false 를 리턴하는 것을 이용하여 while 으로 제어할 수 있다.

```java
while(rs.next()) {
	System.out.println(true라서 실행중); 
}

if(rs.next()) { 
	System.out.println(true라서 실행); 
}
```

- ResultSet [[객체(Object)]]에서 현재 행에서 필드명 혹은 [[레코드(Record)]] 셋에서의 위치를 통해서 어떤 [[Spring/SQL/필드(Field)|필드(Field)]]의 값을 가져올 수 있는데, 이때 getXxx() 메소드를 제공한다.

- 해당 칼럼([[속성(Attribute)]])의 데이터 타입이 문자열이면 getString() 해당 칼럼의 데이터 타입이 int이면 getInt()가 된다.

### getXxx() 종류

#### getString()

#### getDate()

#### getBytes()

#### getDouble()

#### getInt()

#### getTimestamp()

#### getObject()

#### getLong()


## 예시

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
			// Connection 연결 설정
			
			System.out.println("DB연결 성공"); 
			stmt = con.createStatement(); // Statement 만들기
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


- ResultSet은 [[Statement]]를 통해 값을 저장할 수 있다.
- 이때, 사용하는 메서드는 [[executeQuery()]](String sql) [[메서드(Method)]]를 통해 저장할 수 있다.
- [[next()]] 메소드를 통해, 선택되는 행을 바꿀 수 있다.

- 그리고, 다음행이 내려갈 다음행이 있을 경우 TRUE를 반환하고, 없을 경우 FALSE를 반환한다.

![](https://blog.kakaocdn.net/dn/k9YWa/btqEZ9kuQ06/AvAkgE4XORM0mQIi2M5hx0/img.png)

- 위의 이미지에서 커서가 현재 아무행도 가르키지 않고 있다.
- 위 이미지에서 커서가 현재 아무것도 가리키지 않아, 만약 getString()/ getInt()등 Getter [[메서드(Method)]]를 사용했을 경우 에러가 발생한다.

![](https://blog.kakaocdn.net/dn/cadHMb/btqE00fBm5t/yKIF92P6EdtKSFir3QsZ90/img.png)

- 커서가 한칸이 내려갔다.
- 그래서 next() 메서드를 실행하면 위와 같이 커서가 잡하게 된다.
- Geter 메서드를 통해 데이터를 불러올 수 있다.

![](https://blog.kakaocdn.net/dn/vluhM/btqE0xyq3Lo/thGr8TrNnZoKj0RxQMEHN1/img.png)

- 많은 타입을 지정할 수 있다.
- get타입**은 컬럼의 숫자나, 컬럼의 이름을 지정해서 값을 불러 올 수 있게 된다.

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
