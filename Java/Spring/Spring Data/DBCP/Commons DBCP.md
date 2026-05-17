- 스프링 [[Maven]]에서 주로 사용한다.

- [[아파치(apache)]] Commons DBCP 스프링 설정한다.

```xml
// pom.xml
    <bean id="dataSource-mysql"
        class="org.apache.commons.dbcp.BasicDataSource" 
        destroy-method="close">
        <property name="driverClassName" value="${Globals.DriverClassName}"/>
        <property name="url" value="${Globals.Url}" />
        <property name="username" value="${Globals.UserName}"/>
        <property name="password" value="${Globals.Password}"/>
        <property name="maxActive" value="${Globals.maxActive}"/>
        <property name="maxIdle" value="${Globals.maxIdle}"/>
        <property name="maxWait" value="${Globals.maxWait}"/>
    </bean>
```


```xml
<dependency>
    <groupId>commons-dbcp</groupId>
    <artifactId>commons-dbcp</artifactId>
    <version>1.4</version>
</dependency>
```

  
## Commons DBCP 구조

|속성 이름|설명|
|---|---|
|initialSize|BasicDataSource 클래스 생성 후 최초로 getConnection() 메서드를 호출할 때 커넥션 풀에 채워 넣을 커넥션 개수|
|maxActive|동시에 사용할 수 있는 최대 커넥션 개수(기본값: 8)|
|maxIdle|커넥션 풀에 반납할 때 최대로 유지될 수 있는 커넥션 개수(기본값: 8)|
|minIdle|최소한으로 유지할 커넥션 개수(기본값: 0)|

  

[![cp-s1](https://linked2ev.github.io/assets/img/devlog/201908/cp-s2.png)](https://linked2ev.github.io/spring/2019/08/14/Spring-3-%EC%BB%A4%EB%84%A5%EC%85%98-%ED%92%80%EC%9D%B4%EB%9E%80/)

- 8개의 커넥션을 최대로 활용할 수 있는 상태
- 4개는 사용 중이고 4개는 대기 중인 상태

  

커넥션 개수와 관련된 속성은 다음과 같은 조건을 논리적 오류가 없게 설정하는걸 추천한다.

- maxActive >= initialSize
- maxIdle >= minIdle
- maxActive = maxIdle
