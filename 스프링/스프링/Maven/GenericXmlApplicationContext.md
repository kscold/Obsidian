

`GenericXmlApplicationContext`는 스프링 애플리케이션 컨텍스트를 초기화하고 관리하는 역할을 한다.
즉, 스프링 설정 파일을 이용하여 객체를 생성한다.

스프링 애플리케이션은 여러 [[빈(Bean)]]을 관리하고, 
이 빈들 사이의 의존성을 관리하기 위해 컨테이너를 사용합니다. `GenericXmlApplicationContext`는 주로 XML 형식의 스프링 설정 파일을 로드하고, 설정된 빈들을 생성하며, 빈들 사이의 의존성을 해결합니다.

`GenericXmlApplicationContext`를 사용하는 주요 단계는 다음과 같다.

1. **컨테이너 생성**: `GenericXmlApplicationContext` 클래스를 사용하여 스프링 컨테이너를 생성합니다.
    
2. **XML 설정 파일 로드**: 생성된 컨테이너는 XML 형식의 스프링 설정 파일을 로드합니다. 이 설정 파일에는 애플리케이션의 빈 구성 정보가 포함되어 있습니다. 설정 파일은 `classpath:` 접두사를 사용하여 클래스 패스 상에서 찾거나 파일 경로를 지정하여 로드할 수 있습니다.
    
3. **빈(Bean) 생성**: 설정 파일에 정의된 빈들을 컨테이너가 생성하고 초기화합니다. 빈은 클래스 인스턴스일 수도 있고, 팩토리 메서드를 통해 생성될 수도 있습니다.
    
4. **의존성 주입**: 빈들 간의 의존성은 설정 파일에서 지정한 대로 컨테이너에 의해 주입됩니다. 주입 방법에는 생성자 주입, 세터 주입 등이 있으며, 이를 통해 빈들이 협력하고 작동할 수 있습니다.
    
5. **빈 사용**: 생성된 빈들은 애플리케이션에서 사용됩니다. 애플리케이션 코드에서 빈을 가져와 메서드를 호출하거나 필드를 조회하여 작업을 수행할 수 있습니다.
    
6. **컨테이너 닫기**: 애플리케이션이 끝나면 `ctx.close();`를 호출하여 컨테이너를 명시적으로 닫아야 합니다. 이렇게 하면 빈들의 생명주기 관리와 리소스 해제 등의 작업이 수행됩니다.


예시

GenericXmlApplicationContext를 이용하여 스프링 객체를 생성하기 위한 과정으로는

1. src/main/resources 폴더에 applicationContext.xml 스프링 설정 파일을 만든다.
2. 아래의 있는 네이스페이스 관련 설정 태그를 입력한다.
3. bean 태그를 만들고 그 안에 id와 class를 설정한다. id는 객체에 접근하기 위한 key값, class는 패키지명+클래스 네임으로 작성해 준다.
4. GenericXmlApplicationContext 클래스는 이렇게 작성된 스프링 설정 파일을 읽어와 로딩이 시키고, 객체를 생성하며, 초기화하는 역할을 한다. 또한 java 파일에서 getBean 메소드를 이용하여 스프링 컨테이너에 생성된 객체에 접근 가능할 수 있게 된다.

```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
 		http://www.springframework.org/schema/beans/spring-beans.xsd">
        
	<!-- bean definitions here -->
        <bean id="book" class= "lec1Java.Book"/>
        
</beans>
```

```java
packeage lec1Java;

public class Book {
	pubilc void read() {
		System.out.println("책을 읽어요");
	}
}
```


```java
packeage lec1Java;

import org.springframework.context.support.GenericXmlApplicationContext;

public class BookTest {

	pubilc static void main(String[] args) {
		        // 빈을 가져와서 사용

		GenericXmlApplicationContext ctx = new GenericXmlApplicationContext("classpath:applicationContext.xml");
		
        Book book = ctx.getBean("book", Book.class);
		book.read()

        ctx.move();
	}
}
```


![[Pasted image 20231005111135.png]]