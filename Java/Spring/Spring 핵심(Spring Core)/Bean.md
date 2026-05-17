- 빈(Bean)은 [[스프링 컨테이너(Spring Container)]]에 의해 관리되는 재사용 가능한 소프트웨어 [[컴포넌트(Component)]]이다.
- 즉, 스프링 컨테이너가 관리하는 자바 [[객체(Object)]]를 뜻하며, 하나 이상의 빈(Bean)을 관리한다.
- 빈은 [[인스턴스(Instance)]]화된 객체를 의미하며, 스프링 컨테이너에 등록된 객체를 스프링 빈이라고 한다.
- [[@Bean]] 어노테이션을 통해 [[메서드(Method)]]로부터 반환된 객체를 스프링 컨테이너에 등록한다.
- 빈은 [[클래스(Class)]]의 등록 정보, [[Getter and Setter]] 메서드를 포함하며, 컨테이너에 사용되는 설정 메타데이터로 생성된다.

- 설정 메타데이터 : XML 또는 자바 [[어노테이션(Annotation)]], 자바 코드로 표현하며, 컨테이너의 명령과 인스턴스화, 설정, 조립할 객체 등을 정의한다.

## Bean 등록

- 빈으로 등록 될 준비를 하는 것이 무엇일까?  
- 우리가 [[@Controller]], [[@Service]], [[@Component]], [[@Repository]] [[어노테이션(Annotation)]]을 붙인 [[클래스(Class)]]이 빈으로 등록 될 준비를 한 것이다.

## Bean 접근 방법(Maven)

- 먼저, ApplicationContext(스프링 컨테이너)를 사용하여 bean을 정의를 읽고 액세스 할 수 있다.

```java
// create and configure beans
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");

// retrieve configured instance
PetStoreService service = context.getBean("memberRepository", memberRepository.class);

// use configured instance
List<String> userList = service.getUsernameList();
```

- getBean() 메서드를 통해 bean의 [[인스턴스(Instance)]]를 가져올 수 있다.
- ApplicationContext 인터페이스는 bean을 가져오는 여러 가지 방법들이 있다.

- 그러나 실제 응용 프로그램 코드에서는 getBean() 메서드를 통해 호출하지 않는다

## BeanDefinition

- 스프링의 다양한 설정 형식(Java, XML 등)은 BeanDefinition이라는 추상화 덕분에 할 수 있는 것이다.
- Bean은 BeanDefinition(빈 설정 메타정보)으로 정의되고, BeanDefinition에 따라 활용하는 방법이 달라진다.

- BeanDefinition은 속성에 따라 컨테이너가 Bean을 어떻게 생성하고 관리할지를 결정한다.
- @Bean 어노테이션 또는 `<bean>` 태그당 1개씩 메타 정보가 생성된다.

- Spring이 설정 메타정보를 BeanDefinition 인터페이스를 통해 관리하기 때문에 컨테이너 설정을 XML, Java로 할 수 있는 것이다.

- 스프링 컨테이너는 설정 형식이 XML인지 Java 코드인지 모르며, BeanDefinition만 알면 된다.

### BeanDefinition 정보

- BeanClassName : 생성할 빈의 클래스 이름
- factoryBeanName : 팩토리 역할의 빈을 사용할 경우의 이름 (appConfig, … 등)
- factoryMethodName : 빈을 생성할 팩토리 메서드 지정 (memberService, … 등)
- Scope : 싱글톤(기본값)
- lazyInit: 스프링 컨테이너를 생성할 때 빈을 생성하는 것이 아니라, 실제 빈을 사용할 때까지 최대한 생성을 지연 처리하는지 여부
- InitMethodName: 빈을 생성하고, 의존관계를 적용한 뒤에 호출되는 초기화 메서드 명
- DestroyMethodName: 빈의 생명주기가 끝나서 제거하기 직전에 호출되는 메서드 명
- Constructor arguments, Properties: 의존관계 주입에서 사용한다. (자바 설정처럼 팩토리 역할의 빈을 사용하면 없음

### 빈 설정 메타정보 확인하기

```java
AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);

String[] beanDefinitionNames = ac.getBeanDefinitionNames();

for (String beanDefinitionName : beanDefinitionNames ) {
    BeanDefinition beanDefinition = ac.getBeanDefinition(beanDefinitionName);

    if (beanDefinition.getRole() == BeanDefinition.ROLE_APPLICATION) {
        System.out.println("beanDefinitionName = " + beanDefinitionName + ", beanDefinition = " + beanDefinition);
    }
}
```

- Bean 이름 조회 
	- ac.getBeanDefinitionNames(); : 스프링에 등록된 모든 빈 이름을 조회

- Bean 객체 조회
	- ac.getBean(빈이름, 타입) : 빈 인스턴스 조회
	- ac.getBean(타입) : 빈 인스턴스 조회(같은 타입의 스프링 빈이 둘 이상이면 예외 발생)
	- ac.getBeansOfType(타입) : 해당 타입의 모든 빈 조회

- getRole() : 스프링 내부에서 사용하는 빈과 사용자가 등록한 빈을 구분할 수 있다.

- BeanDefinition.ROLE_APPLICATION : 일반적으로 사용자가 정의한 빈
- BeanDefinition.ROLE_INFRASTRUCTURE : 스프링이 내부에서 사용하는 빈