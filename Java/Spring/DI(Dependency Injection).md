- 스프링은 알아서 [[객체(Object)]]를 만들기 때문에 이를 의존 관계 혹은 의존성 주입(Dependency Injection)이라고 부른다.
- 따라서 [[@Autowired]] [[어노테이션(Annotation)]]을 통해 스프링이 미리 생성해 놓은 [[객체(Object)]]를 가져다가 연결해준다.

- 스프링은 3가지 핵심 프로그래밍 모델([[AOP(Aspect-Oriented Programming)]], DI, IOC([[스프링 컨테이너(Spring Container)]])을 지원하는데 그중 하나가 DI이다.

- 주의해야 될 점은 의존성 주입과 [[Bean]] 등록은 다르다. 

- 먼저 [[스프링 컨테이너(Spring Container)]]에 [[Bean]] 으로 등록이 되어야 의존성 주입을 할 수 있다.