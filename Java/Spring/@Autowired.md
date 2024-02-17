- @Autowired란 [[스프링 컨테이너(Spring Container)]]에 등록한 빈([[Bean]])에게 의존 관계 주입이 필요할 때, [[DI(Dependency Injection)]](의존성 주입)을 도와주는 [[어노테이션(Annotation)]]이다.

- [[스프링 컨테이너(Spring Container)]]에 [[Bean]]들을 모두 등록한 후에, 의존성 주입 단계가 이루어진다. 

- 이 때 @Autowired 어노테이션이 부여된 [[메서드(Method)]]가 실행되며 필요한 [[인스턴스(Instance)]]를 주입해준다.

- @Autowired는 총 3가지 방법으로 실현이 가능한데, [[생성자(constructor)]], 수정자(setter), [[Java/필드(Field)|필드(Field)]]를 사용할 수 있다.


## @Autowired의 3가지 구현

- 아래 3가지의 경우에 @Autowired를 사용할 수 있다.

	- [[생성자(constructor)]]
	- [[Getter and Setter]]의 Setter
	- [[Java/필드(Field)|필드(Field)]]

- 그리고 @Autowired는 기본값이 true이기 때문에 의존성 주입을 할 대상을 찾지 못한다면 애플리케이션 구동에 실패한다.(404와 같은 오류가 발생한다.)

### 생성자 주입

- [[생성자(constructor)]]를 통해 의존 관계를 주입하는 방법이다.

- [[객체(Object)]]가 생성될 때 딱 한 번 호출되는 것이 보장된다.
- 의존 관계가 변하지 않는 경우, 필수 의존 관계에 사용한다.

- 의존 관계([[DI(Dependency Injection)]])에 있는 [[객체(Object)]]들을 [[final]]로 선언할 수 있다는 장점이 있고, [[생성자(constructor)]]에서 무조건 설정해주어야 한다.
- 즉, [[생성자(constructor)]]에서 누락이 발생하면 안된다.

- 생성자가 하나일 경우 @Autowired를 생략할 수 있다.

- 밑의 예시 코드처럼 OrderService라는 클래스가 MemberRepository와 DiscountPolicy 클래스에 의존한다고 가정하자.

```java
@Component
public class OrderServiceImpl implements OrderService {
	private final MemberRepository memberRepository; // 한번 초기화 하는 인스턴스 메서드
	private final DiscountPolicy discountPolicy;
	
	@Autowired
	public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) { // 생성자 정의를 통한 의존성 주입
		this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
	}
}
```

- 의존해야하는 객체를 우선 [[private]] [[final]]로 선언한다.
- 그리고 생성자를 통해 해당 객체들을 주입받도록 하고, 생성자의 위에 @Autowired 어노테이션을 추가한다.

### 수정자(Setter) 주입

-  setter를 생성하고, 그 위에 @Autowired를 적는다.

- 스프링 빈([[Bean]])을 모두 등록한 후에 @Autowired가 붙은 수정자를 모두 찾아서 의존관계([[DI(Dependency Injection)]])를 주입한다.
- "선택적"이고, "변화 가능"한 의존 관계에 사용한다.

```java
@Component
public class OrderServiceImpl implements OrderService {
	private MemberRepository memberRepository;
	private DiscountPolicy discountPolicy;
	
	@Autowired
	public void setMemberRepository(MemberRepository memberRepository) {
		// setter 메서드를 통한 의존성 주입
	    this.memberRepository = memberRepository;
	}
	@Autowired
	public void setDiscountPolicy(DiscountPolicy discountPolicy) {
	    this.discountPolicy = discountPolicy;
	}
}
```

- 의존해야하는 객체를 우선 private final로 선언한다.
- 자바 빈 프로퍼티 규약에 따라 해당 변수에 대한 [[setter]] 메서드를 생성하고, 해당 setter에 @Autowired를 붙여서 사용한다.

### 필드 주입

- 제일 간단한 방법이다.
- [[변수(Variable)]]에 @Autowired를 붙여서 사용한다.

- 하지만 단점이 너무 많다. 
- 어플리케이션과 관련이 없는 테스트 코드에서만 한정적으로 사용하자.

```java
@Component
public class OrderServiceImpl implements OrderService {
	@Autowired
	private MemberRepository memberRepository;
	
	@Autowired
	private DiscountPolicy discountPolicy;
}
```

## 생성자 주입을 선택해야하는 이유

1. 불변
    - 대부분의 의존 관계는 어플리케이션 종료까지 변경될 일이 없다.(불변성 유지)
    - 만일 수정자 주입을 사용한다면 [[setter]]를 [[public]]으로 설정해야 한다.(실수로 변경 가능하다면 좋은 방법이 아니다.)
    - 만일 생성자 주입을 사용한다면, 생성자 호출 시점에 1번 호출한다.(즉, 불변하게 설계 가능하다.)

2. 누락
    - 프레임워크 없이 순수한 자바코드를 통해 단위 테스트하는 일이 많다.
    - 수정자 주입을 사용하면 임의의 관련 객체를 만들어야한다. → 의존관계가 한 눈에 보이지 않아 누락 발생 가능
    - 생성자 주입을 사용하면 누락을 막을 수 있다.

3. [[final]] 키워드의 사용
    - 생성자 주입 사용 시 final 키워드 사용 가능하다.(생성자를 통해서만 설정 가능하다.)
    - 최고의 장점으로 컴파일 오류를 통해 “누락”을 놓치지 않을 수 있다.

#### 생성자 주입 정리

1. 생성자 주입은 순수한 자바 언어의 특징을 잘 살리는 방법이다
2. 기본적으로 생성자 주입을 사용하고, 필수값이 아닌 경우 setter 주입 방식을 옵션으로 부여하자. 
	- 생성자 주입과 setter 주입을 동시에 사용할 수 있다.
3. 필드 주입은 사용하지 않도록 한다.