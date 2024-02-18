- 서블릿([[Servlet]])의 생성부터 소멸까지의 [[생명 주기(Life Cycle)]]을 관리하는 역할을 한다.([[스프링 컨테이너(Spring Container)]]의 개념과 비슷하다.)

- 서블릿 컨테이너는 웹 서버와 소켓을 만들고 통신하는 과정을 대신 처리해준다.
- 즉, 개발자는 [[비즈니스 로직(Business Logic)]]만 집중하면 된다.
- 서블릿 [[객체(Object)]]를 [[싱글톤(Singleton)]]으로 관리한다. ([[인스턴스(Instance)]] 하나만 생성하여 공유하는 방식이다.)
	- 상태를 유지(stateful)하게 설계하면 안된다.
	- Thread safety 하지 않는다.

## 서블릿 컨테이너(Servlet Container)가 관리하는 서블릿([[Servlet]])

- 서블릿 컨테이너가 흐름을 제어한다. 
- 즉.  IoC(Inversion of Control)([[스프링 컨테이너(Spring Container)]])가 일어난다.

![](https://velog.velcdn.com/images%2Fyoho98%2Fpost%2F1cb3e791-58be-4fea-9f88-1f0f6b0175ee%2F%EC%84%9C%EB%B8%94%EB%A6%BF%20%EC%83%9D%EB%AA%85%EC%A3%BC%EA%B8%B0%202.png)

1. Servlet Request, Servlet Response 객체를 생성한다. 
2. 설정 파일을 참고하여 매핑할 Servlet을 확인한다.
3. 해당 서블릿 [[인스턴스(Instance)]] 존재의 유무를 확인하여 없으면 init() 메서드(한번만 실행)를 호출하여 생성한다.
4. Servlet Container에 스레드([[Thread]])를 생성하고 service를 실행한다.
5. 응답을 처리하였으면 distory() 메서드를 실행하여(한번만 실행) Servlet Request, Servlet Response 객체를 소멸하고, 서블릿 소멸 시 Garbage Collection(가비지 컬렉션)이 진행된다.

## [[Servlet]] [[인터페이스(Interface)]]

- 서블릿 컨테이너가 서블릿 [[인터페이스(Interface)]]에 있는 [[메서드(Method)]]들을 호출한다.

## Servlet 메서드

- 서블릿 [[생명 주기(Life Cycle)]]와 관련된 [[메서드(Method)]]가 있다.

### [[HttpServlet]]

- 서블릿을 만들기 위해 반드시 [[상속(Inheritance)]]해야 할 필수 [[클래스(Class)]]이다.

### [[HttpServletRequest]]

- 클라이언트가 데이터를 입력하거나 클라이언트의 정보에 대한 요청 값을 가지고 있는 클래스이다.
- 요청에 대한 정보를 가지고 있는 객체이다.

### [[HttpServletResponse]]

- 클라이언트가 요청한 정보를 처리하고 다시 응답하기 위한 정보를 담고 있는 클래스이다.
- 응답에 대한 정보를 가지고 있는 객체이다.

### [[HttpSession]]

- 클라이언트가 세션을 정보를 저장하고 세션 기능을 유지하기 위해서 제공되는 클래스이다.

### 서블릿 기타 [[메서드(Method)]]

#### getServletConfig()

- 서블릿 설정 정보를 다루는 [[ServletConfig]] [[객체(Object)]]를 반환 한다.
- 예시로 서블릿 이름, 서블릿 초기 매개변수 값, 서블릿 환경정보를 들 수 있다.
#### getServletInfo()

- 서블릿을 작성한 사람에 대한 정보이다.
- 예시로 서블릿 버전, 권리를 들 수 있다.