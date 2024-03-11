- 의존성 주입([[DI(Dependency Injection)]])이 완료된 후에 실행되어야 하는 [[메서드(Method)]]에 사용한다.
- 해당 [[어노테이션(Annotation)]]은 다른 리소스에서 호출되지 않아도 수행한다.
- [[생성자(constructor)]] 보다 늦게 호출된다.

## 호출 순서

1. 생성자를 호출한다.
2. 의존성 주입([[DI(Dependency Injection)]]) 완료한다. ([[@Autowired]] || [[@RequiredArgsConstructor]] )
3. @PostConstruct가 실행된다.

## @PostConstruct의 사용 이유

- 생성자가 호출되었을 때, [[Bean]]은 초기화 전이다.
- 즉, [[DI(Dependency Injection)]]가 이루어 지기 전이다.

- @PostConstruct를 사용하면, [[Bean]]이 초기화 됨과 동시에 의존성을 확인할 수 있다.
- [[Bean]] [[생명 주기(Life Cycle)]]에서 오직 한 번만 수행된다.
- 즉, 여러 번 초기화를 방지한다.

## 예시

```java
@Service
public class PostConstructTestService {
	
	@Autowired
    TestMapper testMapper;
	
    // 생성자
    public PostConstructTestService(){
        System.out.println("PostConstructTestService 생성자 호출");
        try {
            testMapper.getTest();
        } catch (Exception e){
            System.out.println("생성자 DI 실패");
        }
    }
    
    @PostConstruct
    public void init(){
        System.out.println("PostConstructTestService @PostConstruct 호출");
        
        try {
            testMapper.getTest();
        } catch (Exception e){
            System.out.println("@PostConstruct DI 실패");
        }
    }
}
```


![](https://velog.velcdn.com/images/limsubin/post/c45baf12-7f6f-424b-855c-f0fc4d4c0490/image.png)