- 스프링 프레임워크에서 제공하는 [[인터페이스(Interface)]]이다.

- 보일러플레이트([[템플릿(Template)]]) 코드 없이 요구사항에 맞게 프레임워크를 조정할 수 있게 해준다.

- 특정한 스프링 [[클래스(Class)]]를 구현하거나 [[상속(Inheritance)]]할 필요 없이 [[MVC(Mode View Controller)]] 구성정보를 제어할 수 있게 해준다.

- @EnableWebMvc 를 통해 활성화된 Web MVC 애플리케이션의 구성정보를 커스터마이징하는 것을 돕기도 한다.
- 스프링 부트에 있는 기본 설정이 마음에 들지 않거나 스프링에 추가적인 설정을 해줄 필요가 있을 때 사용한다.


## 예시
### 인터셉터 등록

- Request를 핸들링 하기 전 후로 처리할 작업이 있을 때 이를 위한 커스텀 인터셉터를 구성하는 용도로 사용할 수 있다.

- 예시로 글로벌 인터셉터를 등록하여 매번 로깅이나 보안 검사를 할 수 있다.

```java
@Configuration
public class MvcConfig implements WebMvcConfigurer {

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new MyInterceptor());
    }
}
```

### View Resolver

- [[컨트롤러(Controller)]]에서 반환된 뷰 이름이 실제 [[뷰(View)]] [[객체(Object)]]로 변환되는 과정을 정의할 수 있다.
- [[뷰(View)]]가 resolve되는 방식을 커스텀한 로직으로 변경할 수 있다.

```java
@Override
public void configureViewResolvers(ViewResolverRegistry registry) {
    InternalResourceViewResolver resolver = new InternalResourceViewResolver();
    resolver.setPrefix("/WEB-INF/views/");
    resolver.setSuffix(".jsp");
    registry.viewResolver(resolver);
}
```

### 리소스 핸들링

- JavaScript, CSS, images 와 같은 정적 리소스를 어떻게 제어할지 구성할 수 있다.
- 리소스의 위치 등을 적절하게 설정해줄 수 있다.

```java
@Override
public void addResourceHandlers(ResourceHandlerRegistry registry) {
    registry.addResourceHandler("/static/**")
            .addResourceLocations("classpath:/static/");
}
```

### 메세지 변환

- [[HTTP]] 메세지 컨버터를 추가하여 [[JSON(Java Script Object Notation)]], XML 과 같은 형식의 데이터를 읽고 쓸 수 있는 메세지 변환기를 등록할 수 있다.

```java
@Override
public void extendMessageConverters(List<HttpMessageConverter<?>> converters) {
    converters.add(new MyCustomMessageConverter());
}
```

### Path Matching 과 Content Negotiation

- Path matching rules 와 content negotation options 를 구성할 수 있다.

```java
@Override
public void configurePathMatch(PathMatchConfigurer configurer) {
    configurer.setUseTrailingSlashMatch(true);
}

@Override
public void configureContentNegotiation(ContentNegotiationConfigurer configurer) {
    configurer.favorPathExtension(false)
              .favorParameter(true)
              .parameterName("mediaType")
              .ignoreAcceptHeader(true)
              .useRegisteredExtensionsOnly(false)
              .defaultContentType(MediaType.APPLICATION_JSON)
              .mediaType("xml", MediaType.APPLICATION_XML);
}
```

### CORS Configuration

- 다른 도메인에 의해 접근되어야 할 때 필요한 [[CORS(Cross-Origin Resource Sharing Policy)]] 규칙을 정의할 수 있다.

```java
@Override
public void addCorsMappings(CorsRegistry registry) {
    registry.addMapping("/**")
            .allowedOrigins("http://allowed-origin.com")
            .allowedMethods("GET", "POST")
            .allowCredentials(true);
}
```

