
`pom.xml`은 Maven 프로젝트의 중요한 구성 파일 중 하나다.

Maven은 Java 기반 프로젝트의 빌드, 종속성 관리 및 프로젝트 관리 도구로 `pom.xml`은 Maven 프로젝트의 설정 파일로, 프로젝트의 메타 정보와 빌드 프로세스를 정의한다. 
"POM"은 "Project Object Model"의 약자로, 프로젝트의 모든 정보를 XML 형식으로 저장하는데 사용된다.

`pom.xml` 파일에는 다음과 같은 주요 요소가 포함된다.


**프로젝트 정보**: 프로젝트의 이름, 버전, 그룹 아이디와 같은 메타 정보를 정의한다.
`
```xml
<groupId>com.example</groupId> 
<artifactId>my-project</artifactId> 
<version>1.0.0</version>
```


**의존성(Dependencies)**: 프로젝트가 의존하는 외부 라이브러리나 모듈을 정의한다.
Maven은 이러한 의존성을 자동으로 다운로드하고 관리한다.

```xml
<dependencies>
	<dependency>
		<groupId>com.example</groupId>
		<artifactId>my-library</artifactId>
		<version>1.0.0</version>
	</dependency>
</dependencies>
```

**빌드 설정**: 빌드 프로세스를 구성하는데 사용된다.
컴파일러 버전, 빌드 플러그인, 리소스 및 출력 디렉토리와 같은 빌드 설정을 지정할 수 있다.

```xml
<build>
	<plugins>
		<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-compiler-plugin</artifactId>
			<version>3.8.1</version>
			<configuration>
				<source>1.8</source>
				<target>1.8</target>
			</configuration>
		</plugin>
	</plugins>
</build>
```

**프로젝트 모듈**: 멀티 모듈 프로젝트를 구성하는 경우, 각 모듈의 관계와 구조를 정의한다.

```xml
<modules>
	<module>module1</module>
	<module>module2</module>
</modules>
```


**플러그인 설정**: 특별한 빌드 작업을 수행하기 위해 플러그인을 구성할 수 있다. 예를 들어, 테스트 실행, 패키징, 배포 작업에 사용된다.

```xml
<plugins>
	<plugin>
	<groupId>org.apache.maven.plugins</groupId>
	<artifactId>maven-surefire-plugin</artifactId>
	<version>2.22.2</version>
	<configuration> <!-- 테스트 설정 --> </configuration>
	</plugin>
</plugins>
```


```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>

    <artifactId>demo</artifactId>

    <version>0.0.1-SNAPSHOT</version>

    <packaging>jar</packaging>

    <name>demo</name>

    <description>Demo project for Spring Boot</description>

    <parent>

        <groupId>org.springframework.boot</groupId>

        <artifactId>spring-boot-starter-parent</artifactId>

        <version>2.0.5.RELEASE</version>

        <relativePath/> <!-- lookup parent from repository -->

    </parent>

    <properties>

        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>

        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>

        <java.version>1.8</java.version>

    </properties>

    <dependencies>

        <dependency>

            <groupId>org.springframework.boot</groupId>

            <artifactId>spring-boot-starter-web</artifactId>

        </dependency>

        <dependency>

            <groupId>org.springframework.boot</groupId>

            <artifactId>spring-boot-devtools</artifactId>

            <scope>runtime</scope>

        </dependency>

        <dependency>

            <groupId>org.springframework.boot</groupId>

            <artifactId>spring-boot-starter-test</artifactId>

            <scope>test</scope>

        </dependency>

    </dependencies>

    <build>

        <plugins>

            <plugin>

                <groupId>org.springframework.boot</groupId>

                <artifactId>spring-boot-maven-plugin</artifactId>

            </plugin>

        </plugins>

    </build>

</project>

```

```xml

위는 Spring boot에서 프로젝트를 생성했을 때 나오는 pom.xml의 내용이다.

pom.xml은 <project>...</project>로 둘러싸여서 section별로 여러 정보를 나타내며 설정할 수 있다.

<modelVersion> : 4.0.0이라고 써있는데 이것은 maven의 pom.xml의 모델 버전이다. 형식이 4.0.0 버전이라고 이해하면 된다.

<groupId> : 프로젝트를 생성한 조직 또는 그룹명으로 보통, URL의 역순으로 지정한다.

<artifactId> : 프로젝트에서 생성되는 기본 아티팩트의 고유 이름이다.

메이븐에 의해 생성되는 일반적인 artifact는 <artifact>-<version>.<extention>이다. (ex demo-0.0.1-SNAPSHOT.jar)

<version> : 애플리케이션의 버전. 접미사로 SNAPSHOT이 붙으면 아직 개발단계라는 의미이며, 메이븐에서 라이브러리를 관리하는 방식이 다르다고 한다.

<packaging> : jar, war, ear, pom등 패키지 유형을 나타낸다.

<name> : 프로젝트 명

<description> : 프로젝트 설명

<url> : 프로젝트를 찾을 수 있는 URL

위와 같은 태그들은 프로젝트 정보에 관련된 내용이다.

<properties> : pom.xml에서 중복해서 사용되는 설정(상수) 값들을 지정해놓는 부분. 다른 위치에서 ${...}로 표기해서 사용할 수 있다. (java.version에 1.8을 적용하고 다른 위치에서 ${java.version}이라고 쓰면 "1.8"이라고 쓴 것과 같다.

<profiles> : dev, prod 이런식으로 개발할 때, 릴리즈할 때를 나눠야할 필요가 있는 설정 값은 profiles로 설정할 수 있다.

```

```xml
<profiles>

  <profile>

   <id>dev</id>

   <properties>

    <java.version>1.8</java.version>

   </properties>

  </profile>

  <profile>

   <id>prod</id>

   <properties>

    <java.version>1.9</java.version>

   </properties>

  </profile>

</profiles>
```

