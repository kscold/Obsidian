- 로직 처리하는 [[서비스(Service)]]를 클래스를 선언할 때 사용하는 @Service [[어노테이션(Annotation)]]이다.
- 서비스 레이어로 내부에서 [[비즈니스 로직(Business Logic)]]을 처리한다.

- 해당 [[어노테이션(Annotation)]]이 선언된 클래스를 서비스로 인식해 서비스 [[객체(Object)]]를 생성한다.
- [[서비스(Service)]] [[클래스(Class)]]에서 @Service [[어노테이션(Annotation)]]을 붙이고 [[컨트롤러(Controller)]]에서 [[객체(Object)]]를 만들면 [[컨트롤러(Controller)]]에서 자동으로 객체 주입([[DI(Dependency Injection)]]) 즉, [[@Autowired]]가 발생하고 이를 통해 서비스 [[객체(Object)]]를 사용할 수 있다.