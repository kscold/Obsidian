- 자바와 데이터베이스와 연결하는 객체인 [[Connection]]을 임시로 생성하고 Class.ofName()를 통해 DriverManager 객체를 생성한다.
- DriverManager.getConnection() [[메서드(Method)]]으로 연결에 성공하면 제대로 [[Connection]] 객체를 생성한다.

## 문법

```java
getConnection(연결문자열, DB_ID, DB_PW)
```

- 연결 문자열(Connection String)은 “jdbc:Driver 종류://IP:포트번호/DB명”의 형식을 가진다. ( 예) jdbc:mysql://localhost:3306/test_db)
- DB_ID는 MySQL과 같은 [[SQL]] 아이디를 뜻한다.
- DB_PW는 MySQ과 같은 [[SQL]] 비밀번호를 뜻한다.
