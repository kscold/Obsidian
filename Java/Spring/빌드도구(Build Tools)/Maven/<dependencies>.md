
`<dependencies/>` 태그에는 프로젝트와 관련된 모든 종속성을 명시합니다. 이러한 종속성은 빌드 프로세스 중에 프로젝트의 클래스 경로를 구성하는 데 사용된다.
현재 프로젝트에 정의된 저장소에서 자동으로 다운로드된다.

[[pom.xml]]에서의 예시

```xml

<?xml version="1.0" encoding="UTF-8"?> <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"> 

... 

	<dependencies> 
		<dependency> 
			<groupId>org.springframework.boot</groupId> 
			<artifactId>spring-boot-starter-web</artifactId>
			<version>2.5.1</version>
		</dependency>
		<dependency> 
			<groupId>org.apache.tomcat</groupId> 
			<artifactId>tomcat-catalina</artifactId> 
			<version>9.0.1</version> 
			<scope>provided</scope> 
		</dependency>
		 <dependency>
			<groupId>junit</groupId> 
			<artifactId>junit</artifactId> 
			<version>4.12</version> 
			<type>jar</type> 
			<scope>test</scope> 
			<optional>true</optional> 
			</dependency> 
		</dependencies> 
		
		... 
		
</project>
```


