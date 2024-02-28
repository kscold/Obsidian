- 스프링 컨테이너는 스프링 프레임워크의 핵심 [[컴포넌트(Component)]]이다.

- 컨테이너는 생명 주기(Life Cycle)를 관리하면 컨테이너라고 부를 수 있다.

- 따라서 스프링 컨테이너는 자바 [[객체(Object)]]의 생명 주기를 관리하며, 생성된 자바 객체들에게 추가적인 기능을 제공한다.
- 스프링에서는 자바 [[객체(Object)]]를 [[Bean]]이라 한다.

- 다른 말로는 IoC(Inversion of Control)(객체를 생성하고 관리하고 책임지고 의존성을 관리해주는 컨테이너)라고도 부른다.

- 즉, 스프링 컨테이너는 내부에 존재하는 빈([[Bean]])의 생명주기를 관리(빈의 생성, 관리, 제거 등)하며, 생성된 빈에게 추가적인 기능을 제공하는 것이다.
- 스프링 컨테이너는 XML, [[어노테이션(Annotation)]] 기반의 자바 설정 [[클래스(Class)]]로 만들 수 있다.

- 스프링 부트(Spring Boot)를 사용하기 이전에는 xml을 통해 직접적으로 설정해 주어야 했지만, 스프링 부트가 등장하면서 대부분 사용하지 않게 되었다.

## 스프링 컨테이너의 종류

![](https://blog.kakaocdn.net/dn/03Xeq/btrPIRfF4Sy/lJtbgmV5bGfRaiOQYxaoxk/img.png)

- 스프링 컨테이너는 Beanfactory와 ApplicationContext 두 종류의 [[인터페이스(Interface)]]로 구현되어 있다.

- 빈 팩토리(Beanfactory)는 빈의 생성과 관계설정 같은 제어를 담당하는 IoC 오브젝트이고, 빈 팩토리를 좀 더 확장한 것이 애플리케이션 컨텍스트(ApplicationContext)이다.
- 애플리케이션 컨텍스트는 IoC 방식을 따라 만들어진 일종의 빈 팩토리로 둘 다 동일한 개념이라 생각하면 된다.
- 주로 사용되는 스프링 컨테이너는 애플리케이션 컨텍스트(ApplicationContext)이다.

## BeanFactory

- 빈 팩토리(BeanFactory)는 스프링 컨테이너의 최상위 [[인터페이스(Interface)]]이다.
- BeanFactory는 빈을 등록, 생성, 조회 등의 빈을 관리하는 역할을 하며, getBean() [[메서드(Method)]]를 통해 빈을 [[인스턴스(Instance)]]화 할 수 있다.
- [[@Bean]] 어노테이션이 붙은 메서드의 이름을 스프링 빈의 이름으로 사용하여 빈 등록을 한다.

## ApplicationContext
- 애플리케이션 컨텍스트(ApplicationContext)는 BeanFactory의 기능을 상속받아 제공한다.
- 따라서, 빈을 관리하고 검색하는 기능을 BeanFactory가 제공하고, 그 외의 부가 기능을 제공한다.

- 부가 기능
    - MessageSource : 메시지 다국화를 위한 인터페이스
    - EnvironmentCapable : 개발, 운영, 환경변수 등으로 나누어 처리하고, 애플리케이션 구동 시 필요한 정보들을 관리하기 위한 인터페이스
    - ApplicationEventPublisher : 이벤트 관련 기능을 제공하는 인터페이스
    - ResourceLoader : 파일, 클래스 패스, 외부 등 리소스를 편리하게 조회

### 스프링 컨테이너의 기능

![](https://blog.kakaocdn.net/dn/b207Em/btrPID23hJc/CF3PKuqZvBVAkpbLu3i8yK/img.png)

- 스프링 컨테이너는 [[Bean]]의 인스턴스화, 구성, 전체 생명 주기 및 제거까지 관리한다.

- 컨테이너는 개발자가 정의한 빈을 객체로 만들어 관리하고 개발자가 필요로 할 때 제공한다.

- 스프링 컨테이너를 통해 원하는 만큼 많은 객체를 가질 수 있다.
- 의존성 주입([[DI(Dependency Injection)]])을 통해 애플리케이션의 컴포넌트를 관리할 수 있다.

- 스프링 컨테이너는 서로 다른 빈을 연결하여 애플리케이션 빈을 연결하는 역할을 한다.

- 개발자는 모듈 간에 의존 및 결합으로 인해 발생하는 문제로부터 자유로울 수 있다.
- 메서드가 언제 어디서 호출되어야 하는지, 메서드를 호출하기 위해 필요한 매개 변수를 준비해서 전달하지 않는다.

### 스프링 컨테이너를 사용하는 이유

- 먼저, 일반 자바에서 [[객체(Object)]]를 생성하기 위해서는 [[new]] 생성자를 사용해야 한다. 
- 그로 인해 애플리케이션에서는 수많은 객체가 존재하고 서로를 참조하게 된다.
- 객체 간의 참조가 많으면 많을수록 의존성이 높아지게 된다.
- 이는 낮은 결합도와 높은 [[캡슐화(encapsulation)]]를 지향하는 객체지향 프로그래밍의 핵심과는 먼 방식이다.
- 따라서, 객체 간의 의존성을 낮추어(느슨한 결합) 결합도는 낮추고, 높은 캡슐화를 위해 스프링 컨테이너가 사용된다.

- 또한, 기존의 방식으로는 새로운 기능이 생기게 되면 변경 사항들을 수작업으로 수정해야 한다.
- 프로젝트가 커질수록 의존도는 높아질 것이고, 그에 따라 코드의 변경도 많아질 것이다.
- 하지만, 스프링 컨테이너를 사용하면 구현 클래스에 있는 의존성을 제거하고 인터페이스에만 의존하도록 설계할 수 있다.

### 스프링 컨테이너의 생성 과정

- 주로 사용하는 설정 방식은 Java의 [[어노테이션(Annotation)]] 기능을 통해 설정하게 된다.
- 하지만 기존 방식인 XML 방식에 대해서도 알아야 한다.

![](https://blog.kakaocdn.net/dn/YstVd/btrPIRNtwTK/KRFHN6m7k2lXRxKts3g981/img.png)

- 기본적으로 스프링 컨테이너는 Configuration Metadata를 사용한다.
- 또한, 스프링 컨테이너는 파라미터로 넘어온 설정 [[클래스(Class)]] 정보를 사용하여 스프링 [[Bean]]을 등록한다.
- 스프링 컨테이너를 만드는 다양한 방법은 ApplicationContext [[인터페이스(Interface)]]의 구현체이다.

- AppConfig.class 등의 구성 정보를 지정하여 스프링 컨테이너를 생성할 수 있다.
- 애플리케이션 클래스는 구성 메타데이터와 결합되어 ApplicationContext를 생성하고 초기화된다.
- 이후 실행 가능한 시스템 또는 애플리케이션을 갖게 된다.

- 만약, 스프링 빈 조회에서 상속관계가 있을 시에는 부모 타입으로 조회하면 자식 타입도 함께 조회된다.
- 즉, Object 타입으로 조회하게 되면 모든 스프링 빈을 조회할 수 있다.

## 스프링 컨테이너 생성과 등록

### 어노테이션을 이용한 방법
- 아래 코드는 자바 [[어노테이션(Annotation)]]을 기반으로 컨테이너를 구성하고 스프링 컨테이너에 등록하는 예시이다.

```java
@Configuration
public class AppConfig {

    @Bean
    public MemberService memberService() {
        return new MemberServiceImpl(memberRepository());
    }

    @Bean
    public MemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }
}
```

- 아래 코드는 어노테이션 기반으로 위 코드에서 구성한 스프링 컨테이너를 생성하는 방법이다.

```java
public class MemberApp {
    public static void main(String[] args) {
        ApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfig.class);
        
        MemberService memberService = applicationContext.getBean("memberService", MemberService.class)
    }
}
```

- [[@ApplicationContext()]]와 [[@Configuration]]를 사용하여 스프링 컨테이너를 생성하고 등록할 수 있다.

### XML을 이용한 방법(Meven)

- XML 기반 구성 메타데이터의 기본 구조는 XML 기반으로 만드는 ClassPathXmlApplicationContext도 있다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
        
    <bean id="..." class="...">  
        <!-- collaborators and configuration for this bean go here -->
     </bean>
        
     <bean id="..." class="...">
         <!-- collaborators and configuration for this bean go here -->
     </bean>
        
     <!-- more bean definitions go here -->
</beans>
```

- `<beans />` 태그를 통해 필요한 값들을 설정할 수 있다.
- `<bean id=”…”>` 태그는 빈 정의를 식별하는 데 사용하는 문자열이다.
- `<bean class=”…”>` 태그는 Bean의 유형을 정의하고 클래스 이름을 사용한다.

- 아래 예시는 XML 방법으로 컨테이너 생성하는 방법이다.

```java

ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");

```

