- BDD(Behavior Driven Development, 행위 주도 개발)는 **사용자의 행동 시나리오 기반으로 테스트를 작성**하는 개발 방법론이다.
- TDD의 확장 개념으로, 기술적 구현 대신 **비즈니스 동작(행위)**에 집중한다.
- 하나의 시나리오는 **Given-When-Then** 구조로 작성한다.

## Given-When-Then 구조

| 단계 | 역할 | 설명 |
| ---- | ---- | ---- |
| **Given** (준비) | 사전 조건 설정 | 테스트 환경과 데이터 준비 |
| **When** (실행) | 행동 수행 | 테스트하려는 동작 실행 |
| **Then** (검증) | 결과 확인 | 예상 결과와 실제 결과 비교 |

## JUnit5 + AssertJ 예시

```java
@DisplayName("주문 서비스 테스트")
class OrderServiceTest {

    private OrderService orderService;
    private FakeOrderRepository orderRepository;

    @BeforeEach
    void setUp() {
        orderRepository = new FakeOrderRepository();
        orderService = new OrderService(orderRepository);
    }

    @Test
    @DisplayName("재고가 충분하면 주문이 성공한다")
    void placeOrder_withSufficientStock_succeeds() {
        // Given (준비)
        Product product = new Product("item-1", 10);   // 재고 10개
        OrderRequest request = new OrderRequest("item-1", 3);  // 3개 주문

        // When (실행)
        Order order = orderService.placeOrder(request);

        // Then (검증)
        assertThat(order.getStatus()).isEqualTo(OrderStatus.CONFIRMED);
        assertThat(order.getQuantity()).isEqualTo(3);
    }

    @Test
    @DisplayName("재고가 부족하면 OutOfStockException이 발생한다")
    void placeOrder_withInsufficientStock_throwsException() {
        // Given
        Product product = new Product("item-1", 1);   // 재고 1개
        OrderRequest request = new OrderRequest("item-1", 5);  // 5개 주문

        // When & Then
        assertThatThrownBy(() -> orderService.placeOrder(request))
            .isInstanceOf(OutOfStockException.class)
            .hasMessage("재고가 부족합니다");
    }
}
```

## TDD vs BDD 비교

| 항목 | TDD | BDD |
| ---- | ---- | ---- |
| 초점 | 코드 단위 테스트 | 비즈니스 행동/시나리오 |
| 작성자 | 개발자 | 개발자 + 기획자 협업 가능 |
| 언어 | 기술적 | 사용자 행동 중심 |
| 테스트 명 | `testCreateUser()` | `사용자가_이메일로_가입하면_가입_완료_이메일을_받는다` |

## BDD 스타일 테스트 네이밍

```java
// 한국어 테스트명 권장 (비즈니스 의도 명확)
@Test
@DisplayName("로그인 실패 시 401 응답을 반환한다")
void login_withWrongPassword_returns401() { ... }

@Test
@DisplayName("관리자만 게시글을 삭제할 수 있다")
void deletePost_onlyAdmin_succeeds() { ... }
```

## 관련

- [[TDD(Test Driven Development)]]
- [[Given-When-Then]]
- [[JUnit5]]
- [[AssertJ]]
- [[@Test]]
- [[@DisplayName]]
