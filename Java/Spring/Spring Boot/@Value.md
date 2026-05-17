- [[프로퍼티 바인딩(Property Binding)]]을 할 수 있는 하나의 방식이다.
 
- @Value("${property}") 방식일 듯 싶지만 불러올 프로퍼티의 갯수가 많거나, 계층형의 구조를 가지고 있는 경우 등에는 적절하지 않다. 
- 또한 프로퍼티명이나 타입을 직접 입력해야 하므로 오류에 대한 여지도 있다.

## 예시
 
- @Value와 Environment 빈에 대한 예시는 아래와 같다.
- 먼저 [[application.yaml]]에 프로퍼티를 yangs.name: brand으로 key-value를 정의한다.

```java
@Component
public class SimpleRunner implements ApplicationRunner {
    @Autowired
    Environment environment; // Environment 타입의 빈 주입
	
    @Value("${yangs.name}") // ${}내부에 작성된 프로퍼티키에 해당하는 값을 세팅
    String name1; 
	
    @Override
    public void run(ApplicationArguments args) throws Exception {
        System.out.println("Value 애노테이션의 yangs.name : " + name1);
		
        String name2 = environment.getProperty("yangs.name");
        System.out.println("Environment bean의 yangs.name : " + name2);
    }
}

// >> Value 애노테이션의 yangs.name : brand
// >> Environment bean의 yangs.name : brand
```
