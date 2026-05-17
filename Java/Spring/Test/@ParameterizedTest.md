- `@ParameterizedTest`는 **[[JUnit5]]에서 여러 입력값으로 같은 테스트를 반복 실행**하는 [[어노테이션(Annotation)]]이다.
- 동일한 로직을 다른 값으로 검증할 때 중복 테스트 메서드 없이 하나로 처리할 수 있다.
- `@Test` 대신 `@ParameterizedTest`를 붙이고, 입력 소스 어노테이션을 추가로 선언한다.

## 입력 소스 종류 비교

| 소스 어노테이션 | 사용 상황 | 예시 |
| ---- | ---- | ---- |
| `@ValueSource` | 단일 타입 값 배열 | strings, ints, longs |
| `@CsvSource` | 여러 컬럼 (CSV 형식) | 입력값 + 기대값 쌍 |
| `@CsvFileSource` | 외부 CSV 파일 | 대용량 테스트 케이스 |
| `@MethodSource` | 복잡한 객체, 스트림 | Stream<Arguments> |
| `@EnumSource` | Enum 값 반복 | 모든 Enum 케이스 |
| `@NullAndEmptySource` | null과 빈 문자열 | 경계값 테스트 |

---

## @ValueSource — 단일 값 반복

```java
@ParameterizedTest
@ValueSource(strings = {"q", "qwerasdfzxcv", "qq23"})
@DisplayName("잘못된 이름 형식은 예외가 발생한다")
void invalidName_throwsException(String invalidName) {
    assertThatThrownBy(() -> new User(VALID_EMAIL, invalidName, password))
        .isInstanceOf(IllegalArgumentException.class);
}

// 숫자 타입
@ParameterizedTest
@ValueSource(ints = {1, 2, 3, 10, 100})
void positiveNumbers_areValid(int number) {
    assertThat(validator.isPositive(number)).isTrue();
}

// 지원 타입: strings, ints, longs, doubles, booleans, chars, bytes, shorts, floats
```

---

## @CsvSource — 여러 컬럼 (입력값 + 기대값)

```java
@ParameterizedTest
@CsvSource({
    "홍길동, 20, true",    // 이름, 나이, 유효 여부
    "김철수, 17, false",
    "이영희, 100, true"
})
@DisplayName("나이에 따라 성인 여부가 다르다")
void checkAdult(String name, int age, boolean expected) {
    assertThat(validator.isAdult(age)).isEqualTo(expected);
}

// 특수문자 포함 시 작은따옴표로 감싸기
@ParameterizedTest
@CsvSource({
    "'Spring, Boot', PUBLISHED",    // 쉼표 포함 제목
    "제목, DRAFT"
})
void parsePost(String title, String status) {
    Post post = postService.create(title, status);
    assertThat(post.getTitle()).isEqualTo(title);
}
```

---

## @MethodSource — 복잡한 객체/스트림

```java
@ParameterizedTest
@MethodSource("provideInvalidCommands")
@DisplayName("잘못된 커맨드는 예외가 발생한다")
void invalidCommand_throwsException(CreatePostCommand command, String expectedMessage) {
    assertThatThrownBy(() -> postService.create(command))
        .isInstanceOf(IllegalArgumentException.class)
        .hasMessageContaining(expectedMessage);
}

// 같은 클래스에 static 메서드로 정의
static Stream<Arguments> provideInvalidCommands() {
    return Stream.of(
        Arguments.of(new CreatePostCommand("", "내용"), "제목은 필수"),
        Arguments.of(new CreatePostCommand("제목", ""), "내용은 필수"),
        Arguments.of(new CreatePostCommand(null, "내용"), "제목은 필수")
    );
}
```

```java
// 다른 클래스에 있는 경우 클래스명#메서드명 지정
@MethodSource("com.example.TestDataFactory#provideInvalidCommands")
```

---

## @EnumSource — Enum 값 반복

```java
enum PostStatus { DRAFT, PUBLISHED, ARCHIVED }

@ParameterizedTest
@EnumSource(PostStatus.class)
@DisplayName("모든 PostStatus로 게시글을 생성할 수 있다")
void createPost_withAllStatuses(PostStatus status) {
    Post post = postService.create("제목", status);
    assertThat(post.getStatus()).isEqualTo(status);
}

// 특정 값만 포함/제외
@ParameterizedTest
@EnumSource(value = PostStatus.class, names = {"PUBLISHED", "ARCHIVED"})
void publishedOrArchivedPost_isVisible(PostStatus status) {
    assertThat(status.isVisible()).isTrue();
}

@ParameterizedTest
@EnumSource(value = PostStatus.class, mode = EnumSource.Mode.EXCLUDE, names = "DRAFT")
void nonDraftPost_canBeIndexed(PostStatus status) {
    assertThat(postIndexer.canIndex(status)).isTrue();
}
```

---

## @NullAndEmptySource — 경계값 테스트

```java
@ParameterizedTest
@NullAndEmptySource    // null과 "" 두 번 실행
@DisplayName("제목이 null이거나 빈 문자열이면 예외가 발생한다")
void nullOrEmptyTitle_throwsException(String title) {
    assertThatThrownBy(() -> postService.create(title, "내용"))
        .isInstanceOf(IllegalArgumentException.class);
}

// @NullSource, @EmptySource 각각 사용도 가능
// @NullAndEmptySource + @ValueSource 조합
@ParameterizedTest
@NullAndEmptySource
@ValueSource(strings = {"   ", "\t", "\n"})   // 공백 문자들도 포함
void blankTitle_throwsException(String title) {
    assertThatThrownBy(() -> postService.create(title, "내용"))
        .isInstanceOf(IllegalArgumentException.class);
}
```

---

## 테스트 이름 커스텀

```java
@ParameterizedTest(name = "[{index}] {0}으로 생성 시 예외 발생")
@ValueSource(strings = {"q", "qwerasdfzxcv", "qq23"})
void invalidName_throwsException(String name) { ... }

// 출력: [1] q으로 생성 시 예외 발생
//       [2] qwerasdfzxcv으로 생성 시 예외 발생
//       [3] qq23으로 생성 시 예외 발생
```

| 플레이스홀더 | 설명 |
| ---- | ---- |
| `{index}` | 현재 테스트 번호 (1부터 시작) |
| `{0}`, `{1}`, ... | 인자 값 |
| `{arguments}` | 모든 인자 |
| `{displayName}` | 테스트 이름 |

---

## 실제 서비스 검증 예시 (BDD 패턴)

```java
@DisplayName("PostService 유효성 검증 테스트")
class PostValidationTest {

    private PostService postService = new PostService(new FakePostRepository());

    @ParameterizedTest
    @CsvSource({
        "1자 제목, false",
        "적절한 제목, true",
        "열다섯자를초과하는긴제목이야, false"
    })
    @DisplayName("제목 길이에 따라 유효성 결과가 다르다")
    void validateTitle(String title, boolean expected) {
        assertThat(postService.isValidTitle(title)).isEqualTo(expected);
    }
}
```

## 관련

- [[JUnit5]]
- [[@Test]]
- [[@DisplayName]]
- [[AssertJ]]
- [[Given-When-Then]]
- [[BDD]]
