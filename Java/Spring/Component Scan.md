- [[Bean]]으로 등록 될 준비를 마친 클래스들을 스캔하여, [[Bean]]으로 등록해주는 것이다.

- component-scan은 기본적으로 [[@Component]] [[어노테이션(Annotation)]]을 [[Bean]] 등록 대상으로 포함한다.  
- 따라서 [[@Controller]] 나 [[@Service]]가 동작하는 이유는 [[@Component]]를 포함하고 있기 때문이다.

## component-scan 사용방법

- component-scan 을 사용하는 방법은 xml 파일에 설정하는 방법과 자바 파일 안에서 설정하는 방법이 있다.

### 1. xml 파일에 설정

```xml
<context:component-scan base-package="com.rcod.lifelog"/> 
```

- 다음과 같이 xml 파일에 설정하고, base package를 적어주면 base package 기준으로 클래스들을 스캔하여 [[Bean]]으로 등록한다.  
- base package에 여러개의 패키지를 쓸 수 있다.

```xml
<context:component-scan base-package="com.rcod.lifelog, com.rcod.example"/> 
```

- 위와 같이 설정하면, base pacakage 하위의 @Controller, @Service, @Repository, @Component 클래스가 모두 빈으로 등록되므로, 특정한 객체만 빈으로 등록하여 사용하고 싶다면 include-filter나 exclude-filter를 통해 설정할 수 있다.

#### exclude-filter

```xml
<context:component-scan base-package="com.rcod.lifelog">
    <context:exclude-filter type="annotation" 
        expression="org.springframework.stereotype.Controller"/>
</context:component-scan>
```

- @Controller 를 제외하고 싶다면 위와 같이 exclude-filter를 사용하여 `org.springframework.stereotype.Controller`를 명시해준다.

#### include-filter

```xml
<context:component-scan base-package="com.rcod.lifelog" use-default="false">
    <context:include-filter type="annotation" 
        expression="org.springframework.stereotype.Controller"/>
</context:component-scan>
```

- `use-default="false"`는 기본 어노테이션 @Controller, @Component등을 스캔하지 않는다는 것이다.  

- 기본 어노테이션을 스캔하지 않는다고 설정하고, `include-filter`를 통해서 위와 같이 특정 어노테이션만 스캔할 수 있다.

### 2. 자바 파일안에서 설정

```java
@Configuration
@ComponentScan(basePackages = "com.rcod.lifelog")
public class ApplicationConfig {
}
```

`@Configuration` 은 이 클래스가 xml을 대체하는 설정 파일임을 알려준다.  
해당 클래스를 설정 파일로 설정하고 `@ComponentScan`을 통하여  
`basePackages`를 설정해준다.

- 위와 같이 `component-scan`을 사용하는 두 가지 방법이 있다.  
    만약 component-scan을 사용하지 않으면, 빈으로 설정할 클래스들을  
    우리가 직접 xml 파일에 일일이 등록해 주어야 한다.

```xml
		<bean id="mssqlDAO" class="com.test.spr.MssqlDAO"></bean>
		
		<!-- MemberList 객체에 대한 정보 전달 및 의존성 주입 -->
		<bean id="member" class="com.test.spr.MemberList">
			
			<!-- 속성의 이름을 지정하여 주입 -->
			<property name="dao">
				<ref bean="mssqlDAO"/>
			</property>
		
		</bean>
```

`MssqlDAO` 와 `MemberList`를 빈으로 등록하고,  
MemberList에 Mssql을 주입한 것이다.  
위와 같이 코드가 매우 길어지고, 일일이 추가하기에 복잡해진다.

## component-scan 동작 과정

ConfigurationClassParser 가 Configuration 클래스를 파싱한다.  
@Configuration 어노테이션 클래스를 파싱하는 것이다.  
                   ⬇  
ComponentScan 설정을 파싱한다.  
base-package 에 설정한 패키지를 기준으로  
ComponentScanAnnotationParser가 스캔하기 위한 설정을 파싱한다.  
                   ⬇  
base-package 설정을 바탕으로 모든 클래스를 로딩한다.  
                   ⬇  
ClassLoader가 로딩한 클래스들을 BeanDefinition으로 정의한다.  
생성할 빈의 대한 정의를 하는 것이다.  
                   ⬇  
생성할 빈에 대한 정의를 토대로 빈을 생성한다.