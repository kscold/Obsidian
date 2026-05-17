- appliaction.properties 파일을 appliaction.yaml로 대체하여 추가할 수 있는 스프링 gradle 세팅이다.


```yaml
debug: false # logback 없이 logging이 가능하지만 너무 많아서 비활성  
management:  
  endpoints:  
    web:  
      exposure:  
        include: "*" # 감춰져있는 엔드포인트를 전부 보이게 만듬  
  
  
logging:  
  level:  
    org.example.boardproject: debug # 현재 프로젝트 디버그  
    org.springframework.web.servlet: debug # 웹 서블릿 패키지로 부터 오는지 조사  
    org.hibernate.type.descriptor.sql.BasicBinder: trace 
    org.hibernate.orm.jdbc.bind: trace # 스프링 3.0부터 sql 로깅이 안되는 문제를 해결하기 위한 설정
  
spring:  
  datasource: # mysql 설정  
    url: jdbc:mysql://localhost:3306/board  
    username: kscold  
    password: Tmdcks6502@  
    driver-class-name: com.mysql.cj.jdbc.Driver 
    # 기본 설정을 위해 외워야되는 h2정보들  
    url: jdbc:h2:mem:testdb 
	username: sa  
	driver-class-name: org.h2.Driver
  jpa:  
    defer-datasource-initialization: true # 테스트용 데이터베이스를 생성  
    hibernate:  
      ddl-auto: create # 엔티티를 보고 자동으로 테이블을 만들어줌  
#    open-in-view: false  
    show-sql: true # sql 쿼리를 보여주는 설정  
    properties: # 스프링 종속성 추가  
      hibernate:  
        format_sql: true # 한줄로 나와야되는 디버그 쿼리문을 이쁘게 포맷팅 해줌  
        default_batch_fetch_size: 100 # jpa에서 복잡한 연관관계 매핑이 되어있는 쿼리를 사용할 때 한번에 벌크로 조인할 수 있게(n+1) 문제를 해결  
  h2:  
    console:  
      enabled: true # 인메모리 데이터 베이스로 h2 DB를 사용할지 설정  
  sql:  
    init:  
      mode: always # data.sql를 언제 작동시킬지 rule를 정하는 설정  
#  thymeleaf:  
#    cache: false  
  
--- # yaml 파일은 새로운 document 생성할 수 있음  
  
spring:  
  config:  
    activate:  
      on-cloud-platform: testdb  
#  datasource:  
#    url: jdbc:h2:mem:board;mode=mysql  
#    driver-class-name: org.h2.Driver  
#  sql:  
#    init:  
#      mode: always  
#  test.database.replace: none
```