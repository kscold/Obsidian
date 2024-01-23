- application.properies 파일에 설정한다.

 ```
# DB URL 설정  

# 유니크 URL 생성하지 않기  
spring.datasource.generate-unique-name=false  

# 고정 URL 설정하기  
spring.datasource.url=jdbc:h2:mem:testdb
```