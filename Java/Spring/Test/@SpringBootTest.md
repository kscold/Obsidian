- 이 [[어노테이션(Annotation)]]을 붙인 [[클래스(Class)]]를 스프링 부트와 연동해 통합 테스트([[테스트 메서드(Test Method)]])를 수행하겠다고 선언하는 것이다.
- 이렇게 하면 테스트 코드에서 스프링 부트가 관리하는 다양한 [[객체(Object)]]를 주입([[DI(Dependency Injection)]])받을 수 있다.

- [[서비스(Service)]] [[클래스(Class)]]를 테스트하기 위해서는 외부에서 [[객체(Object)]]를 주입([[DI(Dependency Injection)]])해야하 하므로 [[@Autowired]]을 붙여주어야 한다.