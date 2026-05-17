- 테스트 메서드는 **[[JUnit5]]의 [[@Test]] [[어노테이션(Annotation)]]이 붙은 메서드**로, 특정 코드의 동작을 자동으로 검증한다.
- [[AssertJ]]의 `assertThat()` 등을 사용해 기대값과 실제값을 비교한다.
- [[@DisplayName]] [[어노테이션(Annotation)]]으로 테스트하고자 하는 바를 명시적으로 나타낸다.

## 기본 구조

```java
@Test
@DisplayName("게시글 생성 시 ID가 부여된다")
void createPost_assignsId() {
    // Given — 사전 조건 설정
    CreatePostCommand command = new CreatePostCommand("제목", "내용");

    // When — 실행
    Post post = postService.create(command);

    // Then — 검증
    assertThat(post.getId()).isNotNull();
    assertThat(post.getTitle()).isEqualTo("제목");
}
```

## 테스트 메서드 명명 규칙

```java
// 방법 1: 메서드명_조건_기대결과 (영어 스네이크 케이스)
void createPost_withValidInput_returnsPost() { }
void findById_notFound_throwsException() { }

// 방법 2: 한글 메서드명 (BDD 스타일, 가독성 높음)
@Test
void 유효한_입력으로_게시글을_생성하면_ID가_부여된다() { }

@Test
void 존재하지_않는_ID로_조회하면_예외가_발생한다() { }
```

## 검증 메서드 종류

```java
// 값 비교
assertThat(result).isEqualTo(expected);
assertThat(result).isNotEqualTo(unexpected);
assertThat(result).isNull();
assertThat(result).isNotNull();

// 숫자 범위
assertThat(age).isBetween(0, 150);
assertThat(score).isGreaterThan(60);
assertThat(price).isLessThanOrEqualTo(10000);

// 문자열
assertThat(name).startsWith("김");
assertThat(email).contains("@");
assertThat(slug).matches("[a-z0-9-]+");

// 컬렉션
assertThat(list).hasSize(3);
assertThat(list).contains("A", "B");
assertThat(list).isEmpty();

// 예외
assertThatThrownBy(() -> service.findById(999L))
    .isInstanceOf(ResourceNotFoundException.class)
    .hasMessageContaining("999");

// 예외 없음
assertThatCode(() -> service.create(command))
    .doesNotThrowAnyException();
```

## 테스트 실패 메시지 커스터마이징

```java
assertThat(post.getStatus())
    .as("게시글 상태가 PUBLISHED여야 합니다")
    .isEqualTo(PostStatus.PUBLISHED);
```

## 관련

- [[@Test]]
- [[@DisplayName]]
- [[AssertJ]]
- [[JUnit5]]
- [[Given-When-Then]]
- [[BDD]]
