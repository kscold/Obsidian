
앞에서 말하길 queryForObject의 **반환형은 데이터형만 가능**하다고 했다.

하지만 말도 안된다.  
그럼 **"SELECT * FROM USER"** 구문으로 **User 객체 자체를 반환받는건 포기**해야 하는걸까?

그걸 위해서 필요한 것이 바로 **RowMapper 인터페이스**이다.

---

## 1. RowMapper란?

RowMapper를 사용하면, **원하는 형태의 결과값을 반환**할 수 있다.

SELECT로 나온 **여러개의 값을 반환**할 수 있을 뿐만 아니라,  
**사용자가 원하는 형태로도 얼마든지** 받을 수 있다.  
즉, 다음과 같이 가능하다는 것이다.

```java
// UserMapper로 인해, User 형태로 반환 가능
List<User> userList = jdbcTemplate.queryForObject(
        "SELECT * FROM USER WHERE id=?",
        UserMapper,
        1000L);
```

이게 어떻게 가능할까?  
우선 잠시 과거로 돌아가보자.

---

## 2. 과거로...

우리가 순수 JDBC를 배울땐 이런 고민을 하지 않았다.  
왜일까?  
그 이유는 우린 **결과값을 ResultSet으로 반환** 받았기 때문이다.

```java
// 쿼리 날리기
ResultSet rs = stat.excuteQuery("SELECT * FROM USER");

// 결과값 가져오기
while(rs.next()) {
     // user 객체에 값 저장
     user = new User();
     user.setId(rs.getInt(1));
     user.setName(rs.getString(2));
     user.setDescription(rs.getString(3));
     
     // 리스트에 추가
     userList.add(user);
}
```

아무런 생각없이 사용했지만, 사실

> - **ResultSet으로 먼저 값을 받고,**
> - **그다음 User 객체에 담아서**
> - **반환!**

이 3가지 단계를 거쳤던 것이다!!

그렇기 때문에 아무 고민없이 결과값을 받을 수 있었다.

---

## 3. RowMapper 사용법

RowMapper의 mapRow 메소드는 이러한 ResultSet을 사용한다.  
사용법은 다음과 같다.

```java
??? mapRow(ResultSet rs, int count);
```

**ResultSet에 값을 담아와서 User 객체에 저장**한다.  
그리고 **그것을 count만큼 반복**한다는 뜻이다.  
(반환형은 뭔지 잘 모르겠다. 확인해봐야 겠지만, 아마 List가 아닐까 싶다)

이것을 실제로 작성해보면 다음과 같다.

```java
public User mapRow(ResultSet rs, int count) throws SQLException {
    // ResultSet 값을 User 객체에 저장
    User user = new User();
    actor.setFirstName(resultSet.getString("name"));
    actor.setLastName(resultSet.getString("description"));
    
    // user 반환
    return user;
};
```

count는 알아서 세어주므로, 작성할 필요없다.

저것을 좀더 깔끔하게 람다식으로 작성하면 다음과 같다.

```java
(rs, count) -> new User(
                     rs.getString("name"),
                     rs.getString("description")
                   )
```

와우! 람다식 어메이징!!

## 3. 향상된 queryForObject

이젠 우린 User 클래스도 반환 받을 수 있다.

```java
// select 구문
String SELECT_BY_ID = "SELECT * FROM USER WHERE id=?";

// user 반환 받기
User user = jdbcTemplate.queryForObject(
        SELECT_BY_ID,
        (rs, count) -> new User(
                     rs.getString("name"),
                     rs.getString("description")
                   ),
        1000L);
```

짠~ : )