- @Autowired란 [[스프링 컨테이너(Spring Container)]]에 등록한 빈에게 의존관계주입이 필요할 때, [[DI(Dependency Injection)]](의존성 주입)을 도와주는 [[어노테이션(Annotation)]]이다.

- 스프링 컨테이너에 [[Bean]]들을 모두 등록한 후에, 의존성 주입 단계가 이루어진다. 

- 이 때 @Autowired 어노테이션이 부여된 [[메서드(Method)]]가 실행되며 필요한 [[인스턴스(Instance)]]를 주입해준다.

- @Autowired는 총 3가지 방법으로 실현이 가능한데, 생성자, 수정자(setter), 필드를 사용할 수 있다.



### 필요한 의존객체 [[DI(Dependency Injection)]]의 “타입"에 해당하는 빈을 찾아 주입

- [[생성자(constructor)]]
- [[setter]]
- [[Java/필드(Field)]]

-> 위의 3가지의 경우에 Autowired를 사용할 수 있다. 그리고 Autowired는 기본값이 true이기 때문에 의존성 주입을 할 대상을 찾지 못한다면 애플리케이션 구동에 실패한다. (404와 같은 오류가 발생한다.)


### 생성자 주입
-  방법 : Constructor 생성자를 통해 의존 관계를 주입하는 방법이다.

- [[객체(Object)]]가 생성될 때 딱 한 번 호출되는 것이 보장된다 → 의존관계가 변하지 않는 경우, 필수 의존관계에 사용

- 의존 관계에 있는 객체들을 final로 선언할 수 있다는 장점 → 생성자에서 무조건 설정해주어야 함 → 누락 발생 X

- 생성자가 하나일 경우 @Autowired를 생략할 수 있다.

예를 들어 OrderService라는 클래스가 MemberRepository와 DiscountPolicy 클래스에 의존한다고 가정하자.

```java
@Component
public class OrderServiceImpl implements OrderService {
	private final MemberRepository memberRepository;
	private final DiscountPolicy discountPolicy;
	
	@Autowired
	public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
            this.memberRepository = memberRepository;
            this.discountPolicy = discountPolicy;
	}
}
```

의존해야하는 객체를 우선 private final로 선언한다.

그리고 생성자를 통해 해당 객체들을 주입받도록 하고, 생성자의 위에 @Autowired 어노테이션을 추가한다.

