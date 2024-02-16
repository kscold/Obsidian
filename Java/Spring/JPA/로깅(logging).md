- 시스템이 작동할 때 당시의 상태와 작동 정보를 서버에 기록할 수 있게 만들어 준다.
 
- 실제 개발을 할때에 println()문을 사용하면 데이터를 바로 출력해 볼 수 있지만, 나중에 따로 찾아볼 수는 없고 성능 또한 저하가 된다. 따라서 로깅을 사용해야 한다.

- 콘솔창에 로깅을 하기 위해서는 [[@SIf4j]](Simple Logging Facade for Java) [[어노테이션(Annotation)]]을 이용하거나 [[SIf4j]] 의존

- [[JPA(Java Persistence API)]] 로깅을 하기 위해서는 application.properies 파일에 logging.level.org.hibernate.SQL=DEBUG 설정을 한다.

```
# JPA 로깅 설정

# 디버그 레벨로 쿼리 출력  
logging.level.org.hibernate.SQL=DEBUG  

# 쿼리 줄바꿈하기  
spring.jpa.properties.hibernate.format_sql=true

# 매개변수 값 보여 주기  
logging.level.org.hibernate.type.descriptor.sql.BasicBinder=TRACE
```


## 로깅 레벨

- 로깅 레벨은 7단계가 있다.
	
	- TRACE(레벨1): DEBUG 레벨보다 더 상세한 정보
	- DEBUG(레벨2): 응용 프로그램을 디버깅하는 데 필요한 세부 정보
	- INFO(레벨3): 응용 프로그램의 순조로운 진행 정보
	- WARN(레벨4): 잠재적으로 유해한 상황 정보
	- ERROR(레벨5):  응용 프로그램이 수행할 수 있는 정도의 오류 정보
	- FATAL(레벨6): 응용 프로그램이 중단될 만한 심각한 오류 정보
	- OFF(레벨7): 로깅 기능 해제

- 보통은 DEBUG로 설정하고 org.hibernate.SQL 로그가 찍히기 시작한다.