- [[Statement]] [[클래스(Class)]]보다 기능이 향상된 [[클래스(Class)]]이다.

- Statement와 PreparedStatement의 차이는 캐시 사용 유무이다.
- statement와 달리 preparedstatement는 [[객체(Object)]]를 캐시에 담아 재사용한다.
- 따라서 반복적으로 [[쿼리(query)]]를 수행한다면 statement에 비해 성능이 좋다.

- 동적인 [[쿼리(query)]]를 처리하는데 적합하다.

- 코드의 안정성과 가독성이 높다.
- 인자 값으로 전달이 가능하다.

## 문법

```java
Connection conn = DriverManager.getConnection("url","id","pw") // Connection 객체로 연결을 얻어옴
PreparedStatement pstmt = conn.prepareStatement("SELECT * FROM TABLE");
ResultSet rs = pstmt2.executeQuery();
```

- 기본 문법은 Statement와 다를게 없지만 [[쿼리(query)]]의 내용을 동적으로 생성해야 할 때 값을 가독성이 좋은 형태로 넣을 수 있다.


## Statement와 PreparedStatement의 차이

### Statement를 사용한 예시

```java
int min_salary = 1000;
String min_id = 110;

stmt = conn.createStatement(); // Statement 객체 생성
stmt.executeQuery("SELECT * FROM EMPLOYEE WHERE SALARY >" + min_salary + "AND EMPLOYEE_ID >" + min_id) // SALARY 열 값이 min_salary 변수에 저장된 값보다 큰 경우와 EMPLOYEE_ID 열 값이 min_id 변수에 저장된 값보다 큰 경우를 비교를 수행
```

- [[변수(Variable)]]나 값을 넣을 때 + 변수 +를 사용하거나 String.format을 사용해서 값을 넣어야 한다.
- 이 방법은 코드 길이는 짧으나 가동성이 좋지 않다.

### PreparedStatement를 사용한 예시

```java
pstmt = conn.preparedStatement("SELECT * FROM EMPLOYEE WHERE SALARY > ? AND EMPLOYEE_ID > ?");
pstmt.setInt(1, min_salary)
pstmt.setString(2, min_id)
```

- [[SQL]]문에 [[변수(Variable)]]나 값을 넣을 때 [[?(Placeholder)]]를 사용한다.   
  
- 값을 넣을 땐 setInt, setString등 변수에 맞는 setXxx 데이터 타입을 사용해서 넣는다.  
- 앞에 숫자는 ?의 순서이다.  

- 코드길이는 길어질 수 있지만 가독성이 좋다.


## 예시

- 밑의 코드는 hr 계정의 employees 테이블에서 salary가 특정 값 이상인 사원 조회하는 [[쿼리(query)]] 예시이다.

```java
// DBConnector 클래스
public class DBConnector {
	public static Connection getConnection() {
		String driver;
		String url;
		String id;
		String password;
		
		try {
			BufferedReader in = new BufferedReader(new FileReader("DB.txt"));
			// DB.txt에 써 있는 정보를 in BufferedReader 객체로 가져옴
			
			// 언패킹
			driver = in.readLine();
			url = in.readLine();
			id = in.readLine();
			password = in.readLine();
			
			Class.forName(driver); // forName()을 통해 드라이버를 설정(DriverManager 객체 생성)
			
			// DriverManager 클래스를 통해 DB와의 연결을 생성한다.
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


```java
// 예시 Class
	public static void main(String[] args) {
		int min_sal = 9000;
		
		try{
			Connection conn = DBConnector.getConnection(); // Connection 객체 설정
            
            // ? 나중에 setInt할 것
			String sql = "SELECT * FROM EMPLOYEES WHERE SALARY > ?";
			
			PreparedStatement pstmt = conn.prepareStatement(sql);
			
            // setInt로 첫번째 ?에 매칭시킬 min_sal 변수 9000값을 대입시킴
			pstmt.setInt(1, min_sal);
			
			// 값을 넣은 후에 executeQuery(), 쿼리를 실행
			ResultSet rs = pstmt.executeQuery(); // 결과를 ResultSet 객체에 담음
            
			// emp_id, first_name, last_name, salary
			while (rs.next()) { // ResultSet에 담긴 객체 레코드가 빌 때까지 반복
				System.out.printf("%d\t%10s\t%10s\t%s\n", rs.getInt(1), rs.getString(2), rs.getString(3), rs.getInt("salary"));
			}
			rs.close(); // ResultSet 객체 정리
			pstmt.close(); // PrepareStatement 객체 정리
			conn.close(); // Connction 객체 정리
		} catch(SQLException e) {
			e.printStackTrace();
		}
	}
```

- employees 테이블의 salary컬럼이 9000보다 높은 사원의 데이터만 조회한다.  
  

## 쿼리문 처리하기 

- PreparedStatement를 사용하고 결과를 내려면 [[executeQuery()]]나 [[executeUpdate()]]를 사용해야 한다.
### [[executeQuery()]]

- db의 데이터를 **조회**할 때 사용한다.
- [[SELECT]], [[SHOW]] 등의 명령어에 사용된다.

- [[ResultSet]]객체를 반환한다. 
- 조회한 정보들이 담겨있다.


```java
String sql = "SELECT * FROM employees";

try (Connection conn = DBConnecter.getConnection(); PreparedStatement pstmt = conn.prepareStatement(sql);) {
	
	// rs에 쿼리 결과값이 담음
	ResultSet rs = pstmt.executeQuery();
	
	while(rs.next()) {
		System.out.printf("%d\t%s\t%s\n",rs.getInt(1),rs.getString(2),rs.getString(3));
	}      
} catch (SQLException e) {
	e.printStackTrace();
}
```

- 사용법은 ResultSet 타입의 [[인스턴스 변수(Instance Variable)]]에 pstmt.executeQuerty()를 해주면 조회한 결과가 담기게 된다.  
- while문으로 반복해 값이 없을때 까지 데이터를 추출할 수 있다.
### [[executeUpdate()]]

- 조회문을 제외한 나머지 쿼리문을 처리한다.
- [[INSERT]], [[UPDATE]], [[DELETE]] 등의 명령어에 사용된다.
- 바뀐 행([[레코드(Record)]])의 수를 리턴한다. 

```java
String sql = "INSERT INTO FRUITS VALUES(fruits_fid_seq.nextval,'Orange','ORANGE')";
String sql2 = "DELETE FRUITS WHERE FRUIT_ID = 10";

try (Connection conn = DBConnecter.getConnection(); PreparedStatement pstmt = conn.prepareStatement(sql); PreparedStatement pstmt2 = conn.prepareStatement(sql2);) {
	
	// 쿼리를 실행하고 변경된 행의 수를 리턴
	pstmt.executeUpdate();
	
	int row = pstmt2.executeUpdate();
	
}catch (SQLException e) {
	e.printStackTrace();
}
```