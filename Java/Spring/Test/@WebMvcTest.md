- Application Context([[스프링 컨테이너(Spring Container)]])를 완전하게 Start 시키지 않고 web layer를 테스트 하고 싶을 때 [[테스트 메서드(Test Method)]]에 @WebMvcTest [[어노테이션(Annotation)]]를 사용하는 것을 고려한다. 
- Present Layer 관련 [[컴포넌트(Component)]]만 스캔을 한다.

- [[서비스(Service)]], [[리파지터리(Repository)]] dependency가 필요한 경우에는 @MockBean으로 주입([[DI(Dependency Injection)]])받아 테스트를 진행 한다. 

- [[@SpringBootTest]]의 경우 모든 빈([[Bean]])을 로드하기 때문에 테스트 구동 시간이 오래 걸리고, 테스트 단위가 크기 때문에 디버깅이 어려울 수 있다.
- [[컨트롤러(Controller)]] 레이어만 슬라이스 테스트 하고 싶을 때에는 @WebMvcTest를 쓰는게 유용하다.

## 특정 Controller 클래스만 지정하여 스캔

```java
@WebMvcTest(ContentController.class)
class ContentControllerTest {
```