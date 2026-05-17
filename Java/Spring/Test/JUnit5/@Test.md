-  [[TDD(Test Driven Development)]] JUnit 프레임워크에서 테스트 메서드임을 나타내는 [[어노테이션(Annotation)]]이다.
- Spring Framework에서도 JUnit을 기반으로 하므로 @Test 어노테이션을 사용하여 테스트 코드를 작성할 수 있다. 
- 이 어노테이션은 메서드에 붙여서 해당 메서드가 테스트 [[메서드(Method)]]임을 나타내며, 해당 메서드가 실행되면서 자동으로 단위 테스트가 수행된다.

## 테스트 메서드 실행 순서

- JUnit은 테스트 메서드를 실행할 때, 메서드 실행 순서를 보장하지 않는다. 
- 이는 다수의 테스트 메서드가 각각의 개별적인 테스트를 수행하기 때문이다.
- 따라서, 테스트 메서드 간의 의존성이 존재하는 경우에는 순서가 보장되지 않기 때문에 문제가 발생할 수 있다.
  
- 이러한 문제를 해결하기 위해서는 @Before, @After, @BeforeClass, @AfterClass 등의 어노테이션을 사용하여 테스트 실행 전/후에 필요한 초기화 작업이나 마무리 작업을 수행하도록 한다.
- 이렇게 함으로써 테스트 메서드 간의 의존성을 최소화할 수 있다.

## 테스트 결과 검증

- JUnit에서는 테스트 결과를 검증하기 위한 다양한 어노테이션을 제공합니다. 주요한 어노테이션은 다음과 같다.
  
@Before: 각각의 테스트 메서드가 실행되기 전에 실행되는 메서드를 정의합니다.  
@After: 각각의 테스트 메서드가 실행된 후에 실행되는 메서드를 정의합니다.  
@BeforeClass: 모든 테스트 메서드가 실행되기 전에 한 번 실행되는 메서드를 정의합니다.  
@AfterClass: 모든 테스트 메서드가 실행된 후에 한 번 실행되는 메서드를 정의합니다.  
@Test: 단위 테스트를 수행하는 메서드를 정의합니다.  
@Ignore: 해당 테스트 메서드를 실행하지 않

@RunWith: JUnit 테스트 실행 방법을 지정합니다.  
@Parameterized: 매개변수화된 테스트를 수행할 수 있도록 합니다.  
@Test(expected = SomeException.class): 해당 테스트가 특정 예외를 발생시켜야 함을 나타냅니다.  
@Test(timeout = 1000): 해당 테스트가 특정 시간 안에 실행되어야 함을 나타냅니다.  
@TestConfiguration: 테스트 설정을 지정하는 클래스임을 나타냅니다.

##   
테스트 실행 방법

Spring Framework에서는 JUnit 프레임워크를 사용하여 테스트를 수행합니다. @Test 어노테이션이 붙은 메서드를 실행하기 위해서는 테스트 클래스에서 @RunWith 어노테이션을 사용하여 SpringRunner 클래스를 지정해야 합니다. 또한, @Autowired 어노테이션을 사용하여 필요한 빈을 주입하고, @MockBean 어노테이션을 사용하여 모의 객체를 생성할 수 있습니다.  
  

```
@RunWith(SpringRunner.class)
@SpringBootTest
public class MyTest {

    @Autowired
    private MyService myService;

    @Test
    public void testMethod() {
        // 테스트할 코드 작성
    }
}
```

이렇게 작성한 테스트 클래스는 testMethod() 메서드가 실행되면서 해당 메서드에서 필요한 빈이나 모의 객체를 사용하여 단위 테스트를 수행합니다. 테스트 결과는 콘솔이