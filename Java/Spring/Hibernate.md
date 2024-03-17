- Hibernate는 자바 언어를 위한 ORM 프레임워크이고 [[JPA(Java Persistence API)]]의 구현체로, [[JPA(Java Persistence API)]] [[인터페이스(Interface)]]를 구현하고 내부적으로 [[JDBC(Java DataBase Connectivity)]] [[API(Application Programming Interface)]]를 사용한다.

- Hibernate 자바 [[객체(Object)]]를 통해 [[데이터베이스(DataBase)]]가 Oracle, MySql, MSSQL 등 에 상관없이 다룰수 있도록 하는 추상화를 목표로 한다.
- Mybatis에 익숙한 사람들이라면, Mybatis 구조가 들어가는 자리에 Hiberante가 들어오는 구조라고 이해하면 된다.

- 이를 통해 개발자는 [[SQL]]를 직접 사용하지 않고 [[메서드(Method)]] 호출만으로 [[쿼리(query)]]가 수행된다.
- 즉, [[SQL]]을 작성하는 시간을 줄여 생산성이 높아진다. 

- 하지만 직접 [[SQL]]을 작성하는것보다는 성능상 좋지 않고 세밀하게 데이터를 조작하기 힘들다.
- 이를 보안하기 위해 [[JPQL(Java Persitence Query Language)]]과 NativeQuery를 지원하고 [[QueryDSL]]을 함께 사용할 수 있다.


# 설정법

- 설정은 스프링 부트, gradle 기준으로 작성하였다.

```java
// build.gradle
dependencies {
	...
    // JPA
	implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    // JDBC
	implementation 'org.springframework.boot:spring-boot-starter-jdbc'
    // MySQL
	runtimeOnly 'mysql:mysql-connector-java'
    ...
}
```

- 아래는 [[application.yaml]] 설정이다.

```r
server:
  port: 8080

spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/devbeekei?characterEncoding=UTF-8&serverTimezone=Asia/Seoul
    username: root
    password: password

  jpa:
    show-sql: true 
    hibernate:
      dialect: org.hibernate.dialect.MySQL5InnoDBDialect
      ddl-auto: validate 
    properties:
      hibernate:
        enable_lazy_load_no_trans: false
        format_sql: true
        use_sql_comments: true
        
        
logging:
  level:
    org:
      hibernate:
        type:
          descriptor:
            sql: trace
```

### jpa.hibernate.dialect  

- [[데이터베이스(DataBase)]] 언어 설정이다.

### jpa.hibernate.ddl-auto

- [[스키마(Schema)]] 자동생성 설정이다.
#### none  

#### update

- 테이블의 내용이 변경된 경우 자동으로 스키마를 업데이트한다.
#### create

- 애플리케이션 구동 시 스키마 create한다.
#### create-drop 

- 애플리케이션 구동 시 스키마 create, 종료 시 drop한다.
#### validate

- 매핑 유효성 검증만 실행한다.

### jpa.properties.hibernate.enable_lazy_load_no_trans

- 영속성 컨텍스트가 종료되어도, 새로운 [[데이터베이스(DataBase)]] 커넥션을 획득해서 [[지연 로딩(Lazy Loading)]]이 가능하도록 설정한다.
- 성능상 false 권장한다.

### jpa.properties.hibernate.format_sql

- [[쿼리(query)]] 출력 시 [[쿼리(query)]]를 가독성 좋게 줄바꾼다.
- jpa.show-sql이 true일 때이다.
#### jpa.properties.hibernate.use_sql_comments

- [[쿼리(query)]] 출력 시 설명 주석 추가한다.
- jpa.show-sql이 true일 때이다.

#### logging.level.org.hibernate.type.descriptor.sql

- [[쿼리(query)]] 출력 시 데이터 값도 출력한다.