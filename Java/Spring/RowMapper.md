- [[queryForObject()]]의 반환형은 데이터형만 가능하다.
- 그러나 RowMapper를 사용하면, 원하는 형태의 결과값을 반환할 수 있다.
- 즉, 데이터 베이스의 반환 결과인 [[ResultSet]]을 [[객체(Object)]] 형태로 변환해준다.

- RowMapper를 사용하면 [[SELECT]]로 나온 여러개의 값을 반환할 수 있을 뿐만 아니라, 사용자가 원하는 형태로도 얼마든지 받을 수 있다.  


## 예시

- 순수 [[JDBC(Java DataBase Connectivity)]]를 배울땐 이런 고민을 할 필요 없이 결과값을 [[ResultSet]]으로 반환 받았다.

```java
// 쿼리 실행
ResultSet rs = stat.excuteQuery("SELECT * FROM USER"); 

// 결과값 가져오기
while(rs.next()) { // 쿼리 결과가 있을 때까지 반복
     // user 객체에 값 저장
     user = new User(); // User 객체 생성
     user.setId(rs.getInt(1)); // 
     user.setName(rs.getString(2));
     user.setDescription(rs.getString(3));
     
     // 리스트에 추가
     userList.add(user);
}
```

- 우의 코드는 사실 아래와 같은 3 단계를 거친다.
	
	 - [[ResultSet]]으로 먼저 값을 받는다.
	 - 그다음 User [[객체(Object)]]에 담는다.
	 - 반환한다.

- 그렇기 때문에 아무 고민없이 결과값을 받을 수 있었다.

- 밑의 코드를 보면 [[JDBC Template]]에서  `SELECT * FROM USER` 구문을 [[queryForObject()]] 사용하면 User [[객체(Object)]] 자체를 반환받을 수 없다.

```java
// UserMapper로 인해, User 형태로 반환 가능
List<User> userList = jdbcTemplate.queryForObject(
    "SELECT * FROM USER WHERE id=?", UserMapper, 1000L 
); // sql
```

 - 따라서 이 때 필요한 것이 바로 RowMapper [[인터페이스(Interface)]]이다.


## mapRow

- RowMapper의 mapRow [[메서드(Method)]]는 이러한 [[ResultSet]]을 사용한다. 

```java
??? mapRow(ResultSet rs, int count);
```

- 첫번째 매개변수는 [[ResultSet]]에 값을 담아와서 ??? 자리의 특정 [[객체(Object)]]에 저장한다.  
- 두번째 매개변수는 그 특정 객체를 count만큼 반복한다는 뜻이다.  

- 이것을 실제로 작성해보면 다음과 같다.

```java
public User mapRow(ResultSet rs, int count) throws SQLException {
	// mapRow로 ResultSet에 값을 담고 ResultSet 값을 User 객체에 저장
    User user = new User();
    actor.setFirstName(resultSet.getString("name"));
    actor.setLastName(resultSet.getString("description"));
    
    // user 반환
    return user;
};
```

- count는 알아서 세어주므로, 작성할 필요없다.

- 위의 코드를 좀더 깔끔하게 [[람다(lambda)]]식으로 작성하면 다음과 같다.

```java
(rs, count) -> new User( // mapRow를 람다식으로 작성
	rs.getString("name"), // 쿼리로 받아온 name 값을 조회
    rs.getString("description") // 쿼리로 받아온 description 값을 조회
)
```


## 향상된 [[queryForObject()]]

- 따라서 아래코드처럼 User [[클래스(Class)]]의 [[인스턴스(Instance)]]([[객체(Object)]])로반환 받을 수 있다.

```java
// select 구문
String SELECT_BY_ID = "SELECT * FROM USER WHERE id=?";

// user 반환 받기
User user = jdbcTemplate.queryForObject(SELECT_BY_ID,
	(rs, count) -> new User(
		rs.getString("name"),
		rs.getString("description")
    ), 1000L
);
```
