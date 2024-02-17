- Statement 클래스보다 기능이 향상된 [[클래스(Class)]]이다.
- Statement와 PreparedStatement의 차이는 캐시 사용 유무이다..
- statement와 달리 preparedstatement는 [[객체(Object)]]를 캐시에 담아 재사용한다.
- 따라서 반복적으로 [[쿼리(query)]]를 수행한다면 statement에 비해 성능이 좋다.

- 코드의 안정성과 가독성이 높다.
- 인자 값으로 전달이 가능하다.

## 문법

```java
Connection conn = DriverManager.getConnection("url","id","pw") // Connection 객체로 연결을 얻어옴
PreparedStatement pstmt = conn.prepareStatement("SELECT * FROM TABLE");
ResultSet rs = pstmt2.executeQuery();
```

기본 문법은 Statement와 다를게 없지만 쿼리의 내용을 동적으로 생성해야 할 때 값을 가독성이 좋은 형태로 넣을 수 있다.

예시

  
**Statement**

```
int min_salary = 1000;
String min_id = 110;

stmt = conn.createStatement();
stmt.executeQuery("SELECT * FROM EMPLOYEE WHERE SALARY >" + min_salary + "AND EMPLOYEE_ID >" + min_id)
```

변수나 값을 넣을 때 ++를 사용하거나 String.format을 사용해서 값을 넣어야 한다.

가독성 ↓  
코드길이 ↓

**PreparedStatement**

```
pstmt = conn.preparedStatement("SELECT * FROM EMPLOYEE WHERE SALARY > ? AND EMPLOYEE_ID > ?");
pstmt.setInt(1, min_salary)
pstmt.setString(2, min_id)
```

- sql문에 변수나 값을 넣을 때 ? 를 사용한다.   
  
- 값을 넣을 땐 setInt, setString등 변수에 맞는 데이터 타입을 사용해서 넣는다.  
  앞에 숫자는 ?의 순서이다.  
  
- 오라클에서 ' ' 를 사용해야하는 값이 있더라도(문자열 등)  '?'가 아닌 ? 만 쓰면 된다.  
  

가독성 ↑  
코드길이 ↑

---

#### 예제

hr 계정의 employees 테이블에서 salary가 특정 값 이상인 사원 조회하는 쿼리  
DBConnector 클래스 사용

**DBConnector**

```
public class DBConnector {
	public static Connection getConnection() {
		String driver;
		String url;
		String id;
		String password;
		
		try {
			BufferedReader in = new BufferedReader(new FileReader("DB.txt"));
			
			driver = in.readLine();
			url = in.readLine();
			id = in.readLine();
			password = in.readLine();
			
			Class.forName(driver);

			// 2. DriverManager 클래스를 통해 DB와의 연결을 생성한다.
			Connection conn = DriverManager.getConnection(url, id, password);

			return conn;
			
		} catch (FileNotFoundException e1) {
			e1.printStackTrace();
			return null;
		} catch (IOException e) {
			e.printStackTrace();
			return null;
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
			return null;
		} catch (SQLException e) {
			e.printStackTrace();
			return null;
		}
	}
}
```

**예제 Class**

```
	public static void main(String[] args) {
		int min_sal = 9000;
		try{
			Connection conn = DBConnector.getConnection();
            // ? 나중에 setInt할 것
			String sql = "SELECT * FROM EMPLOYEES WHERE SALARY > ?";
			PreparedStatement pstmt = conn.prepareStatement(sql);
            // setInt로 값 넣어준다.
			pstmt.setInt(1, min_sal);
			// 값을 넣은 후에 executeQuery()를 한다.
			ResultSet rs = pstmt.executeQuery();
            
			// emp_id, first_name, last_name, salary
			while (rs.next()) {
				System.out.printf("%d\t%10s\t%10s\t%s\n", rs.getInt(1), rs.getString(2), rs.getString(3), rs.getInt("salary"));		
			}
			rs.close();
			pstmt.close();
			conn.close();
		}catch(SQLException e) {
			e.printStackTrace();
		}
	}
```

employees 테이블의 salary컬럼이 9000보다 높은 사원의 데이터만 조회한다.  
  

---

### 쿼리문 처리하기 

  
PreparedStatement를 사용하고 결과를 내려면 executeQuery()나 executeUpdate()를 사용해야 한다.  
  
  

#### executeQuery()

db의 데이터를 **조회**할 때 사용한다. (select, show 등의 명령어)

ResultSet객체를 반환한다. (조회한 정보들이 담겨있다)

  
사용방법

```
 String sql = "SELECT * FROM employees";
try (
	Connection conn = DBConnecter.getConnection(); 
	PreparedStatement pstmt = conn.prepareStatement(sql);
  ) {
	// rs에 쿼리 결과값이 담긴다
	ResultSet rs = pstmt.executeQuery();
	while(rs.next()) {
		System.out.printf("%d\t%s\t%s\n",rs.getInt(1),rs.getString(2),rs.getString(3));
	}      
} catch (SQLException e) {
	e.printStackTrace();
}
```

사용법은 ResultSet 타입의 인스턴스 변수에 pstmt.executeQuerty()를 해주면 조회한 결과가 담기게 된다.  
while문으로 반복해 값이 없을때 까지 뽑을 수 있다.

  
  
  

#### executeUpdate()

조회문을 제외한 나머지 쿼리문을 처리한다. (insert,update,delete 등의 명령어)

바뀐 행의 수를 리턴한다. 

```
 String sql = "INSERT INTO FRUITS VALUES(fruits_fid_seq.nextval,'Orange','ORANGE')";
 String sql2 = "DELETE FRUITS WHERE FRUIT_ID = 10";
try (
	Connection conn = DBConnecter.getConnection(); 
	PreparedStatement pstmt = conn.prepareStatement(sql);
    PreparedStatement pstmt2 = conn.prepareStatement(sql2);
  ) {
	// 쿼리를 실행하고 변경된 행의 수를 리턴한다.
	pstmt.executeUpdate();
	int row = pstmt2.executeUpdate();
}catch (SQLException e) {
	e.printStackTrace();
}
```