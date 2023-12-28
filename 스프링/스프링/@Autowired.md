- @Autowired란 [[스프링 컨테이너(Spring Container)]]에 등록한 빈에게 의존관계주입이 필요할 때, [[DI(Dependency Injection)]](의존성 주입)을 도와주는 [[어노테이션(Annotation)]]이다.

- 스프링 컨테이너에 빈들을 모두 등록한 후에, 의존성 주입 단계가 이루어진다. 

이 때 @Autowired 어노테이션이 부여된 메서드가 실행되며 필요한 인스턴스를 주입해준다.

@Autowired는 총 3가지 방법으로 실현이 가능한데, 생성자, 수정자(setter), 필드를 사용할 수 있다.