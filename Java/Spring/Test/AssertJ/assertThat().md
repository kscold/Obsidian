- `assertThat()`은 **[[AssertJ]]의 핵심 진입점으로, 검증 대상 값을 받아 체이닝 방식으로 다양한 검증을 수행**하는 [[테스트 메서드(Test Method)]]다.
- [[객체(Object)]]의 [[Java/필드(Field)|필드(Field)]]나 [[메서드(Method)]] 호출 결과와 기대값을 비교할 때 주로 사용된다.
- 예외를 기대하지 않는다 — 테스트 메서드 안에서 예외가 발생하면 해당 테스트는 실패한다.

## 기본 값 검증

```java
import static org.assertj.core.api.Assertions.*;

// 동등성
assertThat(result).isEqualTo(expected);
assertThat(result).isNotEqualTo(other);

// null 검사
assertThat(value).isNull();
assertThat(value).isNotNull();

// 타입 검사
assertThat(obj).isInstanceOf(Post.class);

// 체이닝
assertThat(score).isGreaterThan(0)
                 .isLessThanOrEqualTo(100);
```

## 문자열 검증

```java
assertThat("hello world")
    .isNotBlank()
    .startsWith("hello")
    .endsWith("world")
    .contains("lo wo")
    .doesNotContain("abc")
    .matches("[a-z ]+");
```

## 숫자 검증

```java
assertThat(age).isBetween(0, 150);
assertThat(price).isGreaterThan(0);
assertThat(stock).isGreaterThanOrEqualTo(0);
```

## 컬렉션 검증

```java
List<String> list = List.of("A", "B", "C");

assertThat(list)
    .hasSize(3)
    .contains("A", "B")
    .containsExactly("A", "B", "C")   // 순서 포함 정확히 일치
    .doesNotContain("D")
    .isNotEmpty();
```

## hasFieldOrPropertyWithValue()

```java
// 기본 방법 — 필드를 각각 검증 (가독성 저하)
assertThat(member.getNickname()).isEqualTo("김철수");
assertThat(member.getAge()).isEqualTo(30);

// hasFieldOrPropertyWithValue — 하나의 체인으로 개선
assertThat(member)
    .hasFieldOrPropertyWithValue("nickname", "김철수")
    .hasFieldOrPropertyWithValue("age", 30);
```

## extracting()으로 필드 추출 후 검증

```java
List<Post> posts = postService.findAll();

// 단일 필드 추출
assertThat(posts)
    .extracting("title")
    .containsExactly("제목1", "제목2", "제목3");

// 여러 필드 동시 추출
assertThat(posts)
    .extracting("title", "status")
    .containsExactly(
        tuple("제목1", "PUBLISHED"),
        tuple("제목2", "DRAFT")
    );
```

## usingRecursiveComparison()으로 객체 전체 비교

```java
Post expected = new Post("제목", "내용", "DRAFT");
Post actual   = postService.create(command);

// equals() 오버라이드 없이 전체 필드 비교
assertThat(actual)
    .usingRecursiveComparison()
    .ignoringFields("id", "createdAt")
    .isEqualTo(expected);
```

## 문제점 — 첫 번째 실패 시 즉시 종료

```java
// 문제: 첫 번째 검증 실패 → 이후 검증은 실행되지 않음
assertThat(member)
    .hasFieldOrPropertyWithValue("nickname", "nickname")   // 실패 → 즉시 종료
    .hasFieldOrPropertyWithValue("age", 30)                // 실행 안 됨
    .hasFieldOrPropertyWithValue("useAgreement", false);   // 실행 안 됨
```

```
java.lang.AssertionError:
Expecting to have a property or a field named "nickname" with value
  "nickname"
but value was:
  "name"
```

이 문제는 `SoftAssertions`로 해결한다.

```java
// SoftAssertions — 모든 검증을 실행한 뒤 한꺼번에 실패 보고
assertSoftly(softly -> {
    softly.assertThat(member.getNickname()).isEqualTo("김철수");
    softly.assertThat(member.getAge()).isEqualTo(30);
    softly.assertThat(member.isUseAgreement()).isTrue();
});
// 모든 실패가 한 번에 보고됨
```

## 예외 검증

```java
// 예외 발생 검증
assertThatThrownBy(() -> postService.findById(999L))
    .isInstanceOf(ResourceNotFoundException.class)
    .hasMessageContaining("999");

// 예외 없음 검증
assertThatCode(() -> postService.create(validCommand))
    .doesNotThrowAnyException();
```

## 실패 메시지 커스터마이징

```java
assertThat(post.getStatus())
    .as("게시글 상태 검증")
    .isEqualTo(PostStatus.PUBLISHED);
// 실패 시: [게시글 상태 검증] expected: PUBLISHED but was: DRAFT
```

## 관련

- [[AssertJ]]
- [[테스트 메서드(Test Method)]]
- [[@Test]]
- [[JUnit5]]
- [[Given-When-Then]]
