- 스프링부트 프로젝트를 생성하면 외부 프로퍼티 설정파일을 만들게 된다.
- [[application.yaml]], application.properties 등 여기 정의한 key-value들이 결국 어플리케이션이 기동되면서 [[클래스(Class)]]로 바인딩 되는 것을 프로퍼티 파인딩이라고 한다.


## 프로퍼티 바인딩의 종류

- 먼저 클래스에서 외부 프로퍼티 값들을 가져오는 방식을 살펴보자.
- 크게 3가지를 이용할 수 있다.
- 간단한 몇 개의 값이라면 [[@Value]]를 사용하여 바인딩할 수 있지만, 검증이나 Type-safe, 계층형 구조 등을 처리할 수 있는 @ConfigurationProperties를 이용한 방법이 있다.
### [[@Value]]

- 소스코드에서 바로 프로퍼티에 주입받아 사용한다.

### Environment

- 외부설정이 바인딩 된 Environment [[Bean]]을 주입받아 사용한다.

### [[@ConfigurationProperties]]

- 외부설정이 바인딩 될 [[Bean]]([[객체(Object)]])을 생성하여 사용한다.







**요약하면**

- **외부에서 설정된 프로퍼티 파일을 코드에서 사용하는 방법은 여러가지가 있다.**
- **만일 읽어올 프로퍼티가 많거나 계층형이라면 객체에 바인딩하여 사용하는 것을 고려하자.**
- **프로퍼티값을 객체에 바인딩하여 빈으로 등록 후 사용을 위해서는 @ConfigurationProperties(prefix = "test"), @EnableConfigurationProperties, @ConfigurationPropertiesScan({"base.package1", "base.package2"}) 같은 애노테이션을 조합한다.**
- **프로퍼티 바인딩시 검증을 위해서는 @Validated와 javax.validation 애노테이션을 사용한다.**

출처: [https://yangbox.tistory.com/33](https://yangbox.tistory.com/33) [양's World:티스토리]