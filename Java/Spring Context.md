- [[Bean]]의 확장 버전으로 스프링이 [[Bean]]을 다루기 좀 더 쉽도록 기능들이 추가된 공간이다.
- 단순히 Bean을 다루는 것 이외에도 추가적인 기능을 수행한다.

![](https://blog.kakaocdn.net/dn/cF6Ept/btrsMzWzG8F/J9lPrMcPrCwAVBuswYijJK/img.png)

- Bean은 모두 Context안에서 이루어진다.
- Context는 Bean들을 포함하여 여러 기능을 가진 공간이라고 생각하면 편하다.
## 종류

- 여기서 문제가 하나 발생하게 되는데, 자바에는 [[Servlet]]이라는 [[객체(Object)]]가 존재하는 데, 모든 [[객체(Object)]]는 [[Bean]]으로 관리되니, [[Servlet]] 역시 Bean이 존재한다.
- 이러한 서블릿 [[Bean]]들도 각자 공통적인 부분들이 있을 것인데, 서블릿은 모두 독립적인 객체로 서로 간섭(공유)이 불가능하다는 점 때문에 문제가 하나 발생한다.


- 예를 들어 모든 서블릿 Bean마다 공통적인 연결이 들어간다면 굉장히 비효율적일 것이다.
- 예시를 들자면 Animal이라는 부모 클래스 없이 혹은 인터페이스 없이 사자, 호랑이, 개, 고양이 클래스를 구현한다고 가정했을 때, 그럼 위 클래스 개개인마다 '울음' 이라는 공통 메소드를 만들어야하는 데 이는 굉장히 비효율적이다.

- 위와 같은 문제를 해결하고자 Spring Context는 이러한 공통 부분을 서블릿에서 총 2가지의 Context로 분리시켰다.

### 1. ROOT-CONTEXT (공통 부분)

- 모든 [[Servlet]]이 공유할 수 있는 Bean들이 모인 공간이다.
- DB와 관련된 [[리파지터리(repository)]], Service 등이 있다.

### 2. SERLVET-CONTEXT (개별 부분)

- [[Servlet]] 각자의 [[Bean]]들이 모인 공간이다.
- 웹, 앱 마다 한 개씩 존재하므로 웹 앱 그자체를 의미하기도 한다.
- 이 Context 내에서의 Bean들은 서로 공유될 수 없다.
- [[MVC(Mode View Controller)]]의 [[Controller]]가 이에 해당된다.

## Context의 구조

- Context는 종류도 많고 구조도 굉장히 복잡하다.

### 1. Application Context

- 스프링 Context 기능의 중심인 최상위 [[인터페이스(Interface)]]이다.
- 거의 스프링 Context는 이 방법으로 구현하며, 기능에 따라 앞에 `~~ApplicationContext`라고 붙는다.

### 2. AbstractApplication Context

- Application Context가 기능의 중심적인 역할을 수행한다면이 Context는 Application Context를 구현한 [[추상 클래스(Abstract Class)]]로, 내부에 정의된 특수한 [[Bean]]들을 등록할 수 있다.

### 3. GenericApplication Context

- 이름부터 제너릭이듯, **Context로서의 기능을 거의 다** 갖고있다.  
- 주로 수동으로 직접 Bean을 등록할 때 사용한다.  
- XmlBeanDefinitionReader를 사용하여 xml 파일을 읽어와야 한다.  
- 등록 과정이 좀 번거롭다.

### 4. [[GenericXmlApplicationContext]]

- [[Bean]]을 배울 때 보통 가장 먼저 사용하는 [[인터페이스(Interface)]]이다.
- AbstractApplication Context 을 확장한 인터페이스로 Context등록 과정이 간편화되어 있음

1번(Application Context)과 달리 xml 파일을 읽어오는 과정이 내부**에 있으며, 다양한 루트로 설정 파일을 불러올 수 있다.

### 5. ClassPathXmlApplicationContext

- GenericXmlApplicationContext과 비슷하지만, 클래스 경로로 Context를 불러오는 데 특화되어 있다.

### 6. FileSystemXmlApplicationContext

- 말 그대로 클래스 경로가 아닌 실제 파일 경로로 불러온다.
- classPath를 사용하는 것을 권장한다.

## Web Application용 Context 종류

### 1. [[Servlet]] Context

- 자바 자체의 Context를 말한다.
- 스프링도 자바로 만들어졌으므로, 모든 스프링 Context는 ServletContext라고 할 수 있다.

### 2. WebApplicationContext

- 웹 애플리케이션에 특화된 Context이다.
- 앞서 설명한 ROOT, Serlvet Context로 사용된다.

### 3. ConfigurationWebApplicationContext

- WebApplicationContext를 설정하는 데 쓰이는 Context이다.
- WebContext를 설정해야할 때엔 Configurable 클래스로 바꿔서 설정한다.