- 애플리케이션을 배포하지 않고도, 서버의 [[MVC(Mode View Controller)]] 동작을 테스트 할 수 있는 MockMvc의 라이브러리이다. 
- 주로 [[컨트롤러(Controller)]] 레이어 단위테스트에 많이 사용된다.

- 즉, [[컨트롤러(Controller)]] 테스트를 하고싶을 때 실제 서버에 구현한 애플리케이션을 올리지 않고(실제 서블릿 컨테이너([[Servlet Container]])를 사용하지 않고) 테스트용으로 시뮬레이션하여 [[MVC(Mode View Controller)]]가 되도록 도와주는 클래스다


## MockMvc 사용법

- 컨트롤러를 테스트한다고 가정한다.

```java
class ControllerTest{
	
    @Autowired
    private MockMvc mockMvc; // mockMvc 생성
    // 실제로 생성자에서 주입하는 것이 더 좋음
    
    @Test
    public void testController() throws Exception{
    	
        String jjson = "{\"name\": \"부대찌개\"}";
        
        //mockMvc에게 컨트롤러에 대한 정보를 입력하자.
        
        mockMvc.perform(
        get("test?query=food") //해당 url로 요청을 한다.
        .contentType(MediaType.APPLICATION_JSON) // Json 타입으로 지정
        .content(jjson) // jjson으로 내용 등록
        .andExpect(status().isOk()) // 응답 status를 ok로 테스트
        .andDo(print()); // 응답값 print
        )
        
        }
}
```

1. MockMvc를 생성한다.
2. MockMvc에게 요청에 대한 정보를 입력한다.
3. 요청에 대한 응답값을 Expect를 이용하여 테스트한다.
4. Expect가 모두 통과하면 테스트 통과
5. Expect가 1개라도 실패하면 테스트 실패


## 예시

- 아래 코드는 GET 메서드 테스트이고 [[Given-When-Then]] 패턴이 사용되었다.
- 의존성 주입([[DI(Dependency Injection)]])가 일어나야 함으로 [[생성자(constructor)]]에서 [[@Autowired]] [[어노테이션(Annotation)]]을 붙였다.

```java
@DisplayName("View 컨트롤러 - 게시글")  
@WebMvcTest  
class ArticleControllerTest {  
    private final MockMvc mvc;  
	  
    public ArticleControllerTest(@Autowired MockMvc mvc) { // 테스트 패키지에서는 의존성 주입이 필요  
        this.mvc = mvc;  
    }  
	  
    @DisplayName("[view] [GET] 게시글 리스트 (게시판) 페이지 - 정상 호출")  
    @Test  
    public void givenNothing_whenRequestArticlesView_thenReturnsArticlesView() throws Exception {  
        // Given  
	  
        // When & Then        mvc.perform(get("/articles"))  
                .andExpect(status().isOk())  
                .andExpect(content().contentType(MediaType.TEXT_HTML))  
                .andExpect(model().attributeExists("articles"));  
    }  
}

## MockMvc [[메서드(Method)]]

### @ContextHierychy

- 테스트용 [[DI(Dependency Injection)]] 컨테이너 만들 때 [[Bean]] 파일을 지정하는 [[어노테이션(Annotation)]]이다.

```java
@ContextHierarchy({
	@ContextConfiguration(classes = AppConfig.class),
    @ContextConfiguration(classes = WebMvcConfig.class)
})
```

### WebAppConfiguration

- Controller 및 web 환경에 사용되는 빈을 자동으로 생성하여 등록한다.

### standaloneSetup()

- 단독 실행할 때 쓰이는 [[메서드(Method)]]로 MockMvcBuilders 라이브러리에 있어 정적 임포트를 하면 좋다.

```java
	public class WelcomeControllerTest {
    MockMvc mockMvc;

    @Before
    public void setUpMockMvc() {
        this.mockMvc = MockMvcBuilders.standaloneSetup(new WelcomeController()).build();
    }
}
```

- "WelcomController"만 테스트한다는 의미이다. 
- 단위 테스트에서 자주 쓰인다. 
- 대상 Controller를 커스텀 할 수 있다.

### addFilter()

- addFilter()로 필터를 추가할 수 있다.

```java
class testt(){
	
    MockMvc mockMvc;
    
	@Before
	public void setUpMockMvc(){
		
		this.mockMvc = MockMvcBuilders
		    			.standaloneSetup(TestController.class)
                        .addFilter(new SessionFilter()) //필터 추가
                        .build();
	}
}
```

---

### perform()

- MockMvc를 실행할 때는 perform()을 이용하여 설정한 MockMvc를 실행할 수 있다.

```java
@Test
public void testController() throws Exception{
	mockMvc.perform(
        get("test?query=food")
        );
```

- perform에 요청 설정 메서드를 통해성 요청에 대한 설정을 할 수 있다.
- perform에 Expect 메서드를 통해서 테스트를 진행할 수 있다.


### MockMvc Request 설정 메서드

- MockMvc는 요청에 대한 설정을 할 수 있다.  

#### param() / params()

- 쿼리 스트링을 설정한다.
#### cookie()

- 쿠키를 설정한다.
#### requestAttr ()

- 요청 스코프 객체를 설정한다.
#### sessionAttr()

- 세션 스코프 객체를 설정한다.
#### content()

- 요청 본문 설정한다.
#### header() / headers()

- 요청 헤더 설정한다.
#### contentType()

- 본문 타입을 설정한다.

#### contentTypeCompatibleWith()

- 본문 타입을 설정하는 뒤에 부가정보를 비교하지 않는다.
- 예를 들어 :UTF-8과 같은 정보까지 비교하지 않기 때문에 실 개발에서 많이 쓰인다.


```java
@Test
public void testController() throws Exception{
	
    mockMvc.perforem(get("test"))
    	.param("query", "부대찌개")
        .cookie("쿠키 값")
        .header("헤더 값:)
        .contentType(MediaType.APPLICATION.JSON)
        .content("json으로");
}
```


### MockMvc Response 검증 메서드

  - 이제 응답에 대한 검증 메소드를 살펴본다.

#### status 

- 상태 코드를 검증한다.
#### header 

- 응답 header를 검증한다.
#### content

- 응답 본문을 검증한다.
#### cookie

- 쿠키 상태를 검증한다.
#### view

- [[컨트롤러(Controller)]]가 반환한 [[뷰(View)]] 이름을 검증한다.
    
#### redirectedUrl(Pattern)

- 리다이렉트([[Redirect]]) 대상의 경로를 검증한다.
#### model

- 스프링 [[MVC(Mode View Controller)]] 모델 상태를 검증한다.
#### request

- 세션 스코프, 비동기 처리, 요청 스코프 상태를 검증한다.

#### forwardedUrl

- 이동대상의 경로를 검증한다.

```java
@Test
public void testController() throws Exception{
	mockMvc.perform(get("test"))
    	.param("query", "부대찌개")
        .cooke("쿠키 값")
        .header("헤더 값:)
        .contentType(MediaType.APPLICATION.JSON)
        .content("json으로")
        .andExpect(status().isOk()) // 여기부터 검증
        .andExpect(content().string("expect json값"))
        .andExpect(view().string("뷰이름"));
```

- 위킈 코드를 실행하면 status 검증 -> conten 검증 -> view 검증 순으로 실시한다.
- 1개라도 통과되지 않으면 테스트는 fail 하게 된다.


### MockMvc 기타 메서드

- 요청 설정, 검증 설정 메소드가 아닌 기타 메서드도 존재한다.

#### andDo()

- print, log를 사용할 수 있는 메서드이다.
#### print()

- 실행결과를 지정해준 대상으로 출력, default = System.out
#### log()

- 실행결과를 디버깅 레벨로 출력, 레벨은 org.springframework.test.web.servlet.result이다.

```java
@Test
public void testController() throws Exception{
	
    mockMvc.perforem(get("test"))
    	.param("query", "부대찌개")
        .cookie("쿠키 값")
        .header("헤더 값:)
        .contentType(MediaType.APPLICATION.JSON)
        .content("json으로")
        
        .andExpect(status().isOk()) 
        .andExpect(content().string("expect json값"))
        .andExpect(view().string("뷰이름"))
        
        .andDo(print()) //여기부터 기타 메소드
        .andDo(log());
```

### 응답 생태코드

- isOk() : 상태 코드가 200인지 확인
- isNotFount() : 404인지 확인
- isMethodNotAllowed() : 405인지 확인
- isInternalServerError() : 500인지 확인
- is(int status) : 임의로 지정한 상태 코드인지 확인

### 모델 검증 속성

```java
.andExpect(model().atrribute("userId", "fly123"))
```

- attributeExists(String key) : key에 해당하는 데이터가 model에 있는지 검증
- attribute(String key, Object value1) : key에 해당하는 value가 입력해준 "value1"과 일치한지 확인

### 뷰 이름 검증

```java
.andExpect(view().name("home"));
```

- name(String viewName) : 응답된 뷰 이름이 viewName과 일치한지 확인

### 리다이렉트 검증

```java
.andExpect(redirectedUrl("/foodList?name=부대찌개"));
```


