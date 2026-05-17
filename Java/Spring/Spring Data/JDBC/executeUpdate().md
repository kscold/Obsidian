- executeUpdate()는 [[CREATE]], [[DROP]], [[INSERT]], [[DELETE]], [[UPDATE]]와 같은 DML(Data Manipulation Language)에서 실행 결과로 영향을 받은 [[레코드(Record)]](행)의 수를 반환한다.

- executeUpdate()는 반환 타입이 int이므로, 쿼리 실행 결과로 반환되는 값을 int로 받아와야 한다.
- executeUpdate()는 행의 개수를 반환하기 때문에  rs([[ResultSet]])를 사용할 필요 없다.

## 예시

- 밑의 코드는 비밀번호를 변경하는 기능을 구현하기 위한 [[DAO(Data Access Object)]]이다.

- 회원이 입력한 새로운 비밀번호를 DB에 저장한다.
- 이 때 로그인 세션에 기록된 회원번호를 조건으로 DB를 업데이트 하는 [[UPDATE]]문을 실행하게 된다.
- 패스워드는 UNIQUE하지 않아 회원간 중복 될 수 있기 때문에 UNIQUE한 회원 번호로 조건문을 만들어야 한다.

- UPDATE 문의 경우 UPDATE가 성공할 경우 1, 실패할 경우 0을 반환하기 때문에 exeuteUpdate() [[메서드(Method)]]를 사용했고 [[ResultSet]]을 거치치 않고 그 결과값을 바로 int result에 저장했다.

```java
	/* 비밀번호 변경 DAO
	* @param conn
	* @param currentPw
	* @param newPw
	* @param memberNo
	* @return
	* @throws Exception
	*/
	
	public int changePw(Connection conn, String currentPw, String newPw, int memberNo) throws Exception {
		int result = 0; // int 값으로 반환되기 때문에 저장할 변수를 선언
		
		try {
			String sql = prop.getProperty("changePw");
			
			// UPDATE MEMBER SET MEMBER_PW=? WHERE MEMBER_NO=? AND MEMBER_PW=?
			
			pstmt = conn.prepareStatement(sql); // sql 테이블을 불러옴
			pstmt.setString(1, newPw);
			pstmt.setInt(2, memberNo);
			pstmt.setString(3, currentPw);
			
			result = pstmt.executeUpdate();
			
		} finally {
			close(pstmt);
		}
		
		System.out.println("DAO : " + result);
		
		return result;
	}
```