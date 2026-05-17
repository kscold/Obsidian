- `@DisplayName`은 **[[JUnit5]]에서 테스트 [[클래스(Class)]]나 테스트 [[메서드(Method)]]에 사람이 읽기 좋은 이름을 붙이는 [[어노테이션(Annotation)]]**이다.
- `camelCase` 메서드명 대신 한국어나 문장으로 테스트 의도를 명확히 표현할 수 있다.
- IDE나 테스트 리포트에서 결과를 볼 때 가독성이 크게 향상된다.

## 기본 사용

```java
@DisplayName("PostService 테스트")   // 클래스 단위 이름
class PostServiceTest {

    @Test
    @DisplayName("게시글을 생성하면 ID가 부여된다")   // 메서드 단위 이름
    void createPost_success() { ... }

    @Test
    @DisplayName("존재하지 않는 게시글 조회 시 ResourceNotFoundException이 발생한다")
    void findById_notFound_throwsException() { ... }
}
```

## 네이밍 컨벤션

### 한국어 문장형 (BDD 스타일, 권장)

```java
@DisplayName("재고가 충분하면 주문이 성공한다")
@DisplayName("재고가 부족하면 OutOfStockException이 발생한다")
@DisplayName("로그인 실패 시 401 응답을 반환한다")
@DisplayName("관리자만 게시글을 삭제할 수 있다")
```

### [HTTP 메서드][경로] - 결과 형식 (컨트롤러 테스트)

```java
@DisplayName("[GET] /api/posts/{id} - 정상 조회 시 200 OK")
@DisplayName("[POST] /api/posts - 필수 값 누락 시 400 Bad Request")
@DisplayName("[DELETE] /api/posts/{id} - 권한 없음 시 403 Forbidden")
```

### givenCondition_whenAction_thenExpected (영문 메서드명)

```java
// 메서드명: 영문 + @DisplayName: 한국어 조합 권장
@Test
@DisplayName("비밀번호가 최소 8자 미만이면 예외가 발생한다")
void createUser_withShortPassword_throwsException() { ... }
```

## 중첩 클래스와 함께 사용

```java
@DisplayName("PostService 테스트")
class PostServiceTest {

    @Nested
    @DisplayName("게시글 생성")
    class CreatePost {

        @Test
        @DisplayName("정상 입력으로 생성 성공")
        void success() { ... }

        @Test
        @DisplayName("제목 없으면 예외 발생")
        void withEmptyTitle_throwsException() { ... }
    }

    @Nested
    @DisplayName("게시글 조회")
    class FindPost {

        @Test
        @DisplayName("존재하는 ID로 조회 성공")
        void found() { ... }

        @Test
        @DisplayName("없는 ID로 조회 시 예외")
        void notFound_throwsException() { ... }
    }
}
```

## @ParameterizedTest와 함께 사용

```java
@ParameterizedTest(name = "[{index}] {0}은 잘못된 이름 형식이다")
@ValueSource(strings = {"q", "qwerasdfzxcv", "qq23"})
@DisplayName("유효하지 않은 이름으로 생성 시 예외 발생")
void invalidName_throwsException(String invalidName) { ... }
```

## 관련

- [[JUnit5]]
- [[@Test]]
- [[@ParameterizedTest]]
- [[@BeforeEach]]
- [[BDD]]
- [[TDD(Test Driven Development)]]
