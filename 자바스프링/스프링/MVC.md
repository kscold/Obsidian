- MVC는 [[Model]]l-[[View]]-[[Controller]]의 약자이며, 어플리케이션을 구성하는 요소를 역할에 따라 세 가지 모듈로 나누어 구분한 패턴이다.
- 현재 MVC 패턴은 사실 MVC1, MVC2 아키텍쳐에서 발전된 패턴이다.

![[Pasted image 20231101223656.png]]

#### MVC1 패턴
![[Pasted image 20231101224851.png]]
- 브라우저(사용자)로부터 요청이 들어오면 DB로부터 필요한 데이터를 받은 Model 객체(Java Bean)를 JSP 페이지(View)에 담아 응답으로 보내는 패턴이다.

- 위 이미지의 구조를 봤을 때 JSP가 View와 Controller 역할을 모두 담당하기 때문에 JSP Page내에 너무 많은 코드가 들어가 가독성도 떨어질 뿐만 아니라 복잡해질 가능성이 생긴다.

따라서 이러한 점은 보완해 Controller 역할을 하는 [[servlet]](서블릿)이 추가된 MVC2 패턴이 나오게 됐다.

#### MVC2 패턴
![[Pasted image 20231101230101.png]]
- MVC2 패턴의 서블릿은 요청에 대한 비즈니스 로직을 처리한 후, 이를 JSP 파일에 반영하는 역할을 수행한다.

#### Spring MVC
![[Pasted image 20231101230223.png]]

spring framework에서 MVC2 모델을 좀 더 발전시켜 Spring MVC가 나왔으며 이는 MVC2 모델이 기반인 웹 모듈이다.

- 위 이미지의 구조와 역할을 간단히 살펴보면,프론트 컨트롤러(Front Controller)가 우선적으로 클라이언트로부터 모든 요청을 받게 된다.
- 실제 요청의 처리는 개별 컨트롤러 클래스로 위임을 한다.(일종의 중앙처리장치와 같은 것으로 요청이 들어오면 WAS가 개별 서블릿에 해당 요청에 대한 처리를 위임하는 논리와 비슷하다.)
- 개별 컨트롤러 클래스는 핸들러(Handler)라고도 하며, DI를 통해 생성해둔 Bean을 통해 비즈니스 로직 처리 결과를 Model에 담아 다시 프론트 컨트롤러로 보낸다.
- 프론트 컨트롤러는 받은 Model을 알맞은 View 템플릿으로 전달하여 받영시키고, 최종적으로 클라이언트로 보낼 화면을 응답 결과로 전송하는 것이다.


1. 톰캣(WAS) 구동 (web.xml)

스프링 MVC 프로젝트를 구동하면 WAS가 먼저 구동됩니다. 스프링을 사용했다고 해서 기존 서블릿을 이용하던 구조를 사용하지 않는 것은 아닙니다. 스프링 또한 자바의 서블릿 컨테이너 구동 방식 위에서 구동되는 라이브러리 집합체입니다. 따라서 가장 먼저 WAS, 즉 서블릿 컨테이너가 구동됩니다.

1-1. Context Path 설정

톰캣의 "server.xml" 파일을 보면 Context Path와 프로젝트(어플리케이션)의 이름이 매핑되어 있습니다. 이 Path를 달고 들어오는 URL은 해당 프로젝트에 대한 요청이라는 것입니다. 여기서 path를 수정해줄 수 있고, 만약 "/"로 바꿔준다면 별도의 Context path없는 URL로 바로 접속할 수 있게 됩니다.

```
<Context docBase="hssweb" path="/" reloadable="true" source="org.eclipse.jst.jee.server:hssweb"/>
```

1-2. 루트 컨테이너 생성 

구동될 때 참조하는 설정 파일은 프로젝트 안에 있는 "WEB-INF/web.xml" 파일입니다. 먼저 아래 코드는 "루트 컨테이너"에 대한 설정입니다. 루트 컨테이너는 어플리케이션(프로젝트 단위)에 딱 하나만 생기는 최상위 부모 컨테이너입니다. 스프링 컨테이너는 루트 컨테이너가 하나, 그리고 각 서블릿들이 하나씩 가지는 컨테이너, 그리고 개발자가 직접 만드는 컨테이너 세 가지 종류가 있습니다.

보통 루트 컨테이너에서는 웹기술과 관계 없는 자원에 대한 빈(Bean)을 만들어 관리합니다. 디폴트로는 "root-context.xml" 파일을 param으로 제공해주지만 필요 시 새로운 파일을 만들어서 param으로 추가할 수 있습니다. "어플리케이션 컨텍스트" 라고도 부릅니다.

루트와 서블릿용 컨테이너는 스프링 MVC 구조에 따라 설정 파일에 있는 내용대로 자동으로 생성됩니다. 개발자가 xml 설정 파일 외 자바 코드에서 생성에 관여하는 부분은 없습니다. 루트 컨테이너에는 모든 서블릿들에서 공유할 전역적인 설정과 빈(Bean)을 생성해 사용하고, 각 서블릿용 컨테이너에서는 해당 서블릿 고유의 설정과 빈(Bean)을 생성해 사용하게 됩니다. 서블릿용 컨테이너는 루트 컨테이너의 자식이며, 부모 컨테이너로부터 필요한 걸 가져와 사용할 수 있지만 그 반대는 불가합니다.

일단 WAS 구동시에는 루트 컨테이너(어플리케이션 루트 컨텍스트)가 생성됩니다.

![](https://blog.kakaocdn.net/dn/A4gOG/btqCF7QsGfw/8OGBmXRWoAIeONyypdoeW1/img.png)

```xml
	<!-- The definition of the Root Spring Container shared by all Servlets 
		and Filters -->
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>/WEB-INF/spring/root-context.xml</param-value>
	</context-param>

	<!-- Creates the Spring Container shared by all Servlets and Filters -->
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>
```

1-3. URL 매핑

서블릿 컨테이너가 클라이언트로부터 URL 요청을 받았을 때 어떤 서블릿 클래스로 넘겨줄지 매핑해주는 설정입니다. 원래 개발자가 직접 서블릿을 만들었었지만 스프링에서는 "DispatcherServlet" 클래스를 제공해줍니다. 아래 코드는 이 서블릿으로 모든 요청("/")을 매핑해주겠다는 의미이고, 처음 서블릿을 생성할 때 서블릿 컨테이너의 설정 파일인 "servlet-context.xml" 파일을 파라미터로 제공해주겠다는 의미입니다.

아직 요청이 한 번도 안들어왔으므로 서블릿은 생성되지 않고 대기 설정 파일만 가지고 있는 상태입니다.

![](https://blog.kakaocdn.net/dn/BqSW8/btqCLNicYTA/KPDSt5HwokBK9qLh8SyEqK/img.png)

```xml
	<!-- Processes application requests -->
	<servlet>
		<servlet-name>appServlet</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>/WEB-INF/spring/appServlet/servlet-context.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>

	<servlet-mapping>
		<servlet-name>appServlet</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>
```

1-4. 필터 설정 적용

필터 설정은 서블릿으로 요청이 들어가기 전, 그리고 최종 응답 전에 공통적으로 수행되어야 할 기능을 구현해주는 설정입니다. 가장 필수적이면서 많이 쓰이는 용도가 인코딩입니다. 필터 설정 덕분에 기존 서블릿에서 일일이 요청객체마다 처리해주던 인코딩을 별도로 안해줘도 됩니다. 그 외에도 스프링 시큐리티 등 여러 공통 처리에 대한 필터를 설정할 수 있습니다.

![](https://blog.kakaocdn.net/dn/uiqpu/btqCF6cPotK/XSmtgxVLFWKdh8bWwHF320/img.png)

```xml
	<!-- 인코딩 필터 -->
	<filter>
		<filter-name>encodingFilter</filter-name>
		<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
		<init-param>
			<param-name>encoding</param-name>
			<param-value>UTF-8</param-value>
		</init-param>
		<init-param>
			<param-name>forceEncoding</param-name>
			<param-value>true</param-value>
		</init-param>
	</filter>
	<filter-mapping>
		<filter-name>encodingFilter</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>
```

---

2. 클라이언트 요청에 따른 서블릿 구동

이제 서블릿 컨테이너(WAS)가 구동되었으므로 웹 어플리케이션은 클라이언트 요청을 기다리고 있는 상태가 됩니다. 

2-1. DispatcherServlet 로드 및 스프링 컨테이너 생성

첫 요청이 들어오면 서블릿 컨테이너가 URL 매핑된 서블릿을 찾아 메모리에 로드시킵니다. JVM의 코드 영역에 서블릿의 소스코드가 로드되고, 힙 영역에 기타 참조객체가 로드된다고 보면 됩니다. 스프링 MVC 프로젝트에서는 프론트 컨트롤러의 서블릿 역할로 이미 스프링에서 만들어둔 "DispatcherServlet" 클래스를 사용하게 됩니다. 물론 커스터마이징도 가능합니다.

이 서블릿을 처음 구동시키면서 초기화 파라미터(init-param)로 "servelt-context.xml" 파일을 넘겨주고, 서블릿은 이 설정대로 동작하게 됩니다. 필요 시 xml 파일을 추가해서 param으로 추가해줄 수 있습니다. 그리고 위에서 언급했던 서블릿용 컨테이너를 생성해서 서블릿 구동에 필요한 핸들러, 컨트롤러 등의 빈(Bean) 객체를 가집니다. 여러 개의 서블릿을 사용한다면 각자 하나씩 가지게 되는 컨테이너입니다. "서블릿 컨텍스트"라고도 부릅니다. 서블릿 컨테이너라고 하면 WAS와 이름이 겹치기 때문에 햇갈리지 않게 주의해야할 듯합니다.

각 용도의 스프링 컨테이너를 나누는 정확한 용어가 없어서 그냥 서블릿용 컨테이너(서블릿 컨텍스트)라고 부르겠습니다.

![](https://blog.kakaocdn.net/dn/3akcg/btqCKgSQnkW/HODEGqnlZP7zsNfkNTyWnk/img.png)

2-2. sevlet-context.xml의 설정대로 기능 분배

이제 요청을 처리할 구동 절차가 완료됐으므로 서블릿으로 들어온 요청을 적절히 분배해주면 됩니다. 먼저 URL이 매핑된 컨트롤러 클래스를 찾아 요청을 넘겨주는 설정입니다. @Controller, @RequestMapping 등의 어노테이션을 찾아서 처리해주겠다는 의미이고, "component-scan"은 어노테이션을 스캔할 패키지의 범위입니다. 만약 컨틀롤러 클래스가 다른 패키지에도 있다면 아래 설정에서 해당 패키지를 추가해줘야 찾을 수 있습니다.

context 라는 전용 태그는 개발자의 편의를 위한 것입니다. 어노테이션을 스캔해서 빈으로 만들어주는 기능의 여러 클래스들의 빈과 설정들을 자동으로 셋팅해줍니다. 전용태그 없이 bean 설정으로만으로도 가능하지만 설정이 복잡하기 때문에 전용태그를 사용하는 것이 좋습니다. 

![](https://blog.kakaocdn.net/dn/45HRc/btqCLdg4kfz/lD0yIAk7J5lfBYrb2oNAvK/img.png)

```xml
	<!-- Enables the Spring MVC @Controller programming model -->
	<annotation-driven />

	<context:component-scan base-package="com.hsweb.springweb" />
```

css, js, 이미지 등의 정적 리소스는 컨트롤러를 통해 분배하지 않고 클라이언트가 직접 접속해서 가져가도록 합니다. 만약 정적 리소스가 기본 제공되는 "/resources" 폴더 하위에 있지 않고 별도 폴더로 만들었다면 아래 코드에서 해당 폴더 위치를 추가해줘야 합니다.

참고로 "WEB-INF"폴더는 외부에서 직접 접속할 수 없는 폴더이므로 이 폴더 안에 정적 리소스를 넣으면 안됩니다.

![](https://blog.kakaocdn.net/dn/5Efz0/btqCLMwMwUH/e2faKQuU9qZIXN7k5r20q1/img.png)

```
	<resources mapping="/resources/**" location="/resources/" />
```

컨트롤러 클래스의 처리기 메소드가 요청을 처리한 뒤에는 뷰 이름을 String 타입으로 리턴합니다. 리턴된 뷰 페이지 이름은 다시 서블릿이 받아 처리하는데 이 부분을 담당하는 객체가 "ViewResolver" 입니다. 설정 파일에 빈(Bean) 객체로 등록하도록 돼있는데 다른 핸들러처럼 알아서 하지 않고 설정을 따로 넣어주는 이유는 아마 초기화 파라미터를 개발자가 제공하도록 하기 위함으로 보입니다.

기본 설정으로는 뷰 페이지 이름이 들어오면 "prefix + 리턴된 문자열 + .jsp"로 만들어 사용하도록 초기화 파라미터에 "prefix"와 "suffix"를 설정할 수 있습니다. 따라서 개발자 입맛에 맞게 뷰의 루트 폴더를 수정한다거나 하는 등의 수정이 가능해집니다.

그리고 서블릿/JSP 구조와 마찬가지로, 한 번 응답하며 컴파일된 뷰 페이지의 코드는 계속 메모리에 로드된 상태로 재활용하게 됩니다. 서블릿 컨테이너가 계속 가지고 있으면서 사용한다고 생각하면 됩니다.

![](https://blog.kakaocdn.net/dn/HRYz6/btqCKBvIDJK/ONgmkLLBUc7bXwjHd9ahY1/img.png)

```xml
	<beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<beans:property name="prefix" value="/WEB-INF/views/" />
		<beans:property name="suffix" value=".jsp" />
	</beans:bean>
```

---

3. 개발자가 짠 로직 전개

이제 서블릿 구동까지 끝났으니 비로소 개발자가 짜둔 코드에 접근해 로직을 전개합니다. 개발자는 루트 컨테이너에 등록해둔 빈(Bean) 객체를 꺼내쓸 수도 있고, 또는 필요한 컨테이너를 하나 만들어서 빈(Bean)을 생성해 사용할 수도 있습니다.

테스트용이 아니라면 개발자가 직접 컨테이너(컨텍스트)를 생성해서 사용할일이 거의 없을 것 같지만 필요할 경우 직접 코드에서 생성해 사용할 수도 있습니다. 다만 스프링은 순수 자바 코드로 된 POJO 클래스를 지향하는만큼 스프링에 의존적인 코드가 직접 사용되는 것은 그리 권장되지 않습니다.

3-1. 서블릿 컨테이너가 요청을 처리할 스레드(Thread) 생성

서블릿은 멀티 스레드 환경으로 구동됩니다. 하나의 요청을 하나의 스레드에서 처리하는데, WAS(서블릿 컨테이너)가 기동될 때 스레드풀을 생성하고, 알아서 처리해줍니다. 스레드풀 설정은 WAS쪽에서 수정할 수 있는데 여기서는 생략하도록 하겠습니다.

위의 과정들에서 서블릿이 구동되며 생겨난 컨테이너와 객체들은 모두 싱글톤 패턴으로, 모든 스레드에서 공유할 수 있는 객체들입니다. 따라서 지금 생성된 스레드는 개발자가 객체를 새로 생성하지 않는 이상 별도의 객체를 가지고 있지 않는 상태라고 볼 수 있습니다.

따라서 컨트롤러 클래스의 처리기 메소드 코드 또한, 객체를 생성하는게 아니라 기존 객체를 참조해 사용하게 됩니다. 해당 메소드에서 생성하는 지역변수를 저장할 스택 프레임만 메모리에 할당됩니다.

![](https://blog.kakaocdn.net/dn/wiYtZ/btqCLdOVEO9/umakf25HYoHUAo1G9Ghuq0/img.png)

3-2. 메소드 안에서 개발자용 컨테이너 생성 및 빈(Bean) 객체 사용

따로 명칭이 있는지는 잘 모르겠지만 루트 컨테이너와 서블릿용 컨테이너와 헷갈리므로 그냥 개발자용 컨테이너라고 부르겠습니다.

일단 메소드 안에서 개발자용 컨테이너를 생성할 경우, 일반 변수와 마찬가지로 스택 프레임 내의 지역 변수로 생성됩니다. 이 말은 메소드가 리턴되는 순간 컨테이너도 사라진다는 의미이며, 컨테이너의 최대 생존 시간은 스레드의 생존 시간과 같다는 의미입니다. 컨테이너도 그냥 하나의 객체입니다. 

실제 컨테이너와 빈(Bean) 객체는 힙 메모리에 저장되겠지만 스택 프레임 안에 있는 참조를 잃는 순간 GC(가비지 컬렉터)의 대상이 됩니다. 이 경우 close() 메소드를 통해 직접 컨테이너 자원해제를 해주지 않으면 결국 잦은 GC를 유발하게 됩니다. 잦은 GC는 성능에 악영향을 미치고, 이 때문에 객체의 재활용이 더욱 중요합니다.

컨테이너를 활용한 빈(Bean) 객체 사용은 의존성 주입 설정을 통해 조금 더 편하게 객체를 생성할 수 있게 하는 것도 있겠지만, 객체를 싱글톤 패턴으로 딱 하나만 생성해 메모리에 로드시킨 뒤 계속 재사용하자라는 취지가 크므로 개인적으로 메소드 안에 지역변수로 할당하는 것은 딱히 컨테이너와 빈(Bean)을 사용하는 의미가 없지 않나 싶습니다. 

![](https://blog.kakaocdn.net/dn/Cm2Z4/btqCGP2Z2KG/eEa1JVEWjoMU4Qjk3cAOeK/img.png)

실제 테스트를 위해 아래와 같이 지역변수로 컨테이너와 빈을 생성해보면 매번 요청이 올 때마다 메모리 주소가 달라지는 것을 볼 수 있습니다. 그리고 close()를 하지 않았으므로 생성되는 객체들은 계속 GC의 대상이 될 것입니다.

![](https://blog.kakaocdn.net/dn/c0DXj2/btqCGR7GyFc/QwY1TgWdVlFMQ6zvkKK3xk/img.png)

![](https://blog.kakaocdn.net/dn/cpZFVZ/btqCKA4GB61/XxGe3qfw4sfX1b9WQC352K/img.png)

```java
package com.hsweb.springweb;

import org.springframework.context.support.GenericXmlApplicationContext;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.servlet.ModelAndView;

@Controller
@RequestMapping(value = "/board")
public class HomeController {

	// "/list" URI에 대한 요청 처리
	@RequestMapping(value = "/list")
	public String home2() {
		
		// 메소드 안에서 컨테이너와 빈 생성 
		GenericXmlApplicationContext ctx = new GenericXmlApplicationContext(
				"classpath:appCTX.xml");
		Test test = ctx.getBean("test", Test.class);
		System.out.println("Test ---- " + test);
		System.out.println("CTX -----" + ctx);
		return "/board/list";
	}
}
```

3-3. 컨트롤러 클래스의 필드멤버로 개발자용 컨테이너 생성 및 빈(Bean) 객체 사용

위에서 언급한 문제를 해결하기 위한 방법입니다. 컨트롤러 클래스는 서블릿용 컨테이너에서 참조하는 빈(Bean) 객체가 되어 어플리케이션이 종료될 때까지 GC의 대상이 되지 않습니다. 따라서 컨트롤러 클래스의 필드 멤버로 컨테이너를 만들 경우, 이 컨테이너 또한 별도로 close()를 해줄 때까지 참조를 잃지 않을 수 있습니다.

또한 컨트롤러 클래스 자체가 딱 한번 만들어져 사용되는 싱글톤 객체이기 때문에, 필드 멤버 또한 딱 한번 만들어져 재활용됩니다. 개발자용 컨테이너 또한 마찬가지로 딱 한개만 만들어져 사용되고, 이 안에서 참조하는 빈(Bean) 객체 또한 하나만 만들어진다는 뜻입니다. 결국 모든 요청 처리 과정에서 하나의 빈(Bean) 객체만을 사용할 수 있기 때문에 보다 효율적으로 자원을 사용할 수 있다는 장점이 생깁니다.

컨테이너와 Bean에 대한 활용은 대부분 @Component 계열의 어노테이션을 통해 루트 컨테이너에 등록해 사용하는 경우가 많기 때문에 이렇게 직접 생성해서 사용할만한 경우가 많지는 않을 것 같습니다. 

![](https://blog.kakaocdn.net/dn/bLVi6x/btqCLduDG4f/SWPACFUUKhl01rufGks0kk/img.png)

위의 구조로 개발자용 컨테이너를 사용했을 경우, 요청이 아무리 많이 들어와도 동일한 컨테이너와 빈(Bean) 객체를 사용하고 있는 것을 확인할 수 있습니다. 

![](https://blog.kakaocdn.net/dn/RTM3i/btqCLMDyUv8/PpzlYyO565r3D0OKZdWUDK/img.png)

```java
package com.hsweb.springweb;

import org.springframework.context.support.GenericXmlApplicationContext;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.servlet.ModelAndView;

@Controller
@RequestMapping(value = "/board")
public class HomeController {

	// 필드 멤버로 컨테이너와 빈(bean) 객체 생성
	GenericXmlApplicationContext ctx = new GenericXmlApplicationContext(
			"classpath:appCTX.xml");

	// "/list" URI에 대한 요청 처리
	@RequestMapping(value = "/list")
	public String home2() {
		
	 
		Test test = ctx.getBean("test", Test.class);
		System.out.println("Test ---- " + test);
		System.out.println("CTX -----" + ctx);
		return "/board/list";
	}
}
```

---

스프링 MVC는 대략 위와 같은 구조로 동작합니다. 테스트를 제외하면 개발자가 직접 컨테이너(컨텍스트)를 생성하고 사용할 일은 없으므로 개념과 구조만 잘 알아두면 될 것 같습니다.