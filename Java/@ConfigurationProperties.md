
## 에시
 
-  @ConfigurationProperties(prefix = “test”)로 [[프로퍼티 바인딩(Property Binding)]]을 하는 예시이다.

- 프로퍼티가 맵핑될 클래스(아래의 TesterProperties)를 작성하고 @ConfigurationProperties을 마킹한다. 
- 이 클래스는 프로퍼티값이 맵핑될 클래스라는 의미로 [[어노테이션(Annotation)]]에는 프로퍼티명의 prefix(아래의 prefix = "test")를 지정할 수 있다.

- 프로퍼티 파일에서 리스트형으로 작성한 sub-testers를 맵핑할 수 있도록 List`<Subster>` subTesters 로 선언한 것을 볼 수 있고, 작성된 위의 프로퍼티 파일은 kebab case(- 가 구분자)로 작성하였으나 클래스 변수명은 camel case이다.

- 스프링부트에서는 프로퍼티명의 camel case, kebab case, underscore notation 간의 변환을 모두 지원한다.
- 즉, subTesters - sub-testers - sub_testers 모두 변환하여 바인딩이 가능하다.

```java
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

import java.util.ArrayList;
import java.util.List;

// 읽어들인 프로퍼티 중 해당 prefix의 프로퍼티값을 바인딩
@ConfigurationProperties(prefix = "test")
public class TesterProperties {
    private String name;
    private int cnt;
    private boolean isMainTester;
    private List<SubTester> subTesters = new ArrayList<>();

    // Getter, Setter, ToString 생략
    
    static class SubTester {
        private String name;
        private int cnt;
        private boolean isMainTester;
            
        // Getter, Setter, ToString 생략
    }
}
```

### @EnableConfigurationProperties()

- 이제 바인딩이 완료된 TesterProperties를 빈으로 등록하여 사용하기 위해 @EnableConfigurationProperties를 마킹한다.
- @EnableConfigurationProperties(TesterProperties.class)로 프로퍼티 [[클래스(Class)]] 사용할 수 있다.

- 주로 프로퍼티 값을 사용할 @Configuration 파일에 마킹하며, @ConfigurationProperties가 마킹된 클래스를 빈으로 등록해주는 역할이다.
- 만일 프로퍼티 클래스가 다수라면 @ConfigurationPropertiesScan({"base.package1", "base.package2"}) 으로 여러 패키지를 스캔하여 등록할 수도 있다.

- 프로퍼티가 제대로 바인딩 됬는지 테스트를 위해 ApplicationRunner를 작성 후 콘솔로그를 찍어본다.

```java
import org.springframework.boot.ApplicationArguments;
import org.springframework.boot.ApplicationRunner;
import org.springframework.boot.context.properties.EnableConfigurationProperties;
import org.springframework.stereotype.Component;

@Component
@EnableConfigurationProperties(TesterProperties.class)
public class PropertyRunner implements ApplicationRunner {
    private final TesterProperties testerProperties; // 값이 바인딩된 빈이 의존성 주입된다.

    public PropertyRunner(TesterProperties testerProperties) {
        this.testerProperties = testerProperties;
    }

    @Override
    public void run(ApplicationArguments args) throws Exception {
        // 바인딩이 제대로 되었는지 콘솔 출력
        System.out.println("=================================");
        System.out.println("TesterProperties : " + testerProperties);
        System.out.println("=================================");
    }
}

// >> =================================
// >> TesterProperties : TesterProperties{name='tester', cnt=99, isMainTester=true, 
// >> subTesters=[SubTester{name='sub-tester1', cnt=11, isMainTester=false},
// >> SubTester{name='sub-tester2', cnt=22, isMainTester=false}]}
// >> =================================
```

- String, int, boolean 및 List 형까지 바인딩이 제대로 된 것을 확인할 수 있다. 
- 혹여 바인딩시 Validation을 하고 싶다면 아래와 같이 스프링의 @Validated 와 JSR-303에 정의된 javax.validation 애노테이션을 사용할 수 있다.

```java
@ConfigurationProperties(prefix = "test")
@Validated // 검증대상임을 표시
public class TesterProperties {
    @NotNull
    private String name;
    @Max(90)
    private int cnt;
    private boolean isMainTester;
    @Valid // 내부 프로퍼티에 검증이 적용되려면 @Valid를 마킹한다.
    private List<SubTester> subTesters = new ArrayList<>();

    static class SubTester {
        @NotNull
        private String name;
        @Min(20)
        private int cnt;
        private boolean isMainTester;
    }
    
    // Getter, Setter, ToString 생략
}
```