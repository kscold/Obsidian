- 미리 일정량의 DB Connenction을 생성해서 풀에 저장해 두고 있다가 [[HTTP]] 요청에 따라 필요할 때 풀에서 커넥션을 가져다 사용하는 기법이다.
- DB와 커넥션을 맺고 있는 객체를 관리하는 역할을 한다.

- 참고로 스프링 부트 2.0 부터는 default Connection Pool로 HikariCP 사용한다.

- 웹 컨테이너([[WAS(Web Application Sever)]])가 실행되면 DB와 미리 connection(연결)을 해놓은 [[객체(Object)]]들을 [[Pool]]에 저장해둔다.
- 클라이언트 요청이 오면 connection을 빌려주고, 처리가 끝나면 다시 connection을 반납받아 pool에 저장하는 방식을 말한다.

- [[JDBC(Java DataBase Connectivity)]]의 단점을 보완하기 위해 커넥션 풀을 사용한다.

[![cp-s1](https://linked2ev.github.io/assets/img/devlog/201908/cp-s1.png)](https://linked2ev.github.io/spring/2019/08/14/Spring-3-%EC%BB%A4%EB%84%A5%EC%85%98-%ED%92%80%EC%9D%B4%EB%9E%80/)

## 커넥션 풀(DBCP)을 사용하는 이유


```java
Connection conn = null;
PreparedStatement  pstmt = null;
ResultSet rs = null;

try {
    sql = "SELECT * FROM T_BOARD"
    
    // 1. 드라이버 연결 DB 커넥션 객체를 얻음
    connection = DriverManager.getConnection(DBURL, DBUSER, DBPASSWORD);

    // 2. 쿼리 수행을 위한 PreparedStatement 객체 생성
    pstmt = conn.createStatement();

    // 3. executeQuery: 쿼리 실행 후
    // ResultSet: DB 레코드 ResultSet에 객체에 담김
    rs = pstmt.executeQuery(sql);
    } catch (Exception e) {
    } finally {
        conn.close();
        pstmt.close();
        rs.close();
    }
}
```

- 위와 같이 자바에서 DB에 직접 연결해서 처리하는 경우(JDBC) 드라이버(Driver)를 로드하고 커넥션(connection) [[객체(Object)]]를 받아와야 한다.

- 그러면 매번 사용자가 요청을 할 때마다 드라이버를 로드하고 커넥션 객체를 생성하여 연결하고 종료하기 때문에 매우 비효율적이다. `
- 이런 문제를 해결하기 위해서 커넥션풀(DBCP)를 사용한다.
### DBCP의 장점
  
1. [[WAS(Web Application Sever)]]와 데이터 베이스와의 연결(Connection)을 줄여서 비용을 줄인다.
2. [[Pool]]내에 미리 연결(Connection)되어 있기에 매번 생성하는 비용을 줄일 수 있다.
  3. 연결(Connection)에 대해서 조정을 할 수 있다. (단, 인프라의 DB Connection 수와 맞추어야 한다.)  
  
- Connection Pool을 크게 설정하면 메모리 소모가 큰 대신 많은 사람의 대기시간이 줄어든다.
- Coonection Pool을 적게 설정하면 사용자의 대기시간이 길어진다.

## 커넥션 풀(DBCP) 특징

- 웹 컨테이너([[WAS(Web Application Sever)]])가 실행되면서 connection 객체를 미리 pool에 생성해 둔다.
- HTTP 요청에 따라 pool에서 connection객체를 가져다 쓰고 반환한다.
- 이와 같은 방식으로 물리적인 데이터베이스 connection(연결) 부하를 줄이고 연결 관리 한다.
- pool에 미리 connection이 생성되어 있기 때문에 connection을 생성하는 데 드는 요정 마다 연결 시간이 소비되지 않는다.
- 커넥션을 계속해서 재사용하기 때문에 생성되는 커넥션 수를 제한적으로 설정한다.

## 동시 접속자가 많을 경우

- 위에 커넥션 풀 설명에 따르면, 동시 접속 할 경우 pool에서 미리 생성 된 connection을 제공하고 없을 경우는 사용자는 connection이 반환될 때까지 번호순대로 대기상태로 기다린다.

- 여기서 [[WAS(Web Application Sever)]]에서 커넥션 풀을 크게 설정하면 메모리 소모가 큰 대신 많은 사용자가 대기시간이 줄어들고, 반대로 커넥션 풀을 적게 설정하면 그 만큼 대기시간이 길어진다.

## 커녁션 풀 사용 시 유의 사항

- 커넥션의 사용 주체는 WAS 스레드이므로 커넥션 개수 WAS 스레드 수와 함께 고려해야 한다.
- 커넥션 수를 크게 설정하면 메모리 소모가 큰 대신 동시 접속자 수가 많아지더라도 사용자 대기 시간이 상대적으로 줄어들게 되고, 반대로 커넥션 개수를 작게 설정하면 메모리 소모는 적은 대신 크만큰 대기시간이 길어질 수 있다.
- 따라서 적정량의 커넥션 객체를 생성해 두어야 한다.

## [[아파치(apache)]]의 DBCP의 종류

- 커넥션 풀의 종류는 Commons DBCP와 Tomcat-JDBC, BoneCP, [[HikariCP]] 등이 있다.
### [[Commons DBCP]] 

### [[HikariCP]] 