- 자바 표준에는 `Arrays.of()`라는 [[메서드(Method)]]는 존재하지 않는다.
- 일반적으로 비슷한 의도로 사용되는 정적 팩토리 메서드는 `Arrays.asList()`, `List.of()`, `Set.of()`, `Map.of()`이다.

## Arrays.asList()

- 가변 인자([[String]]... 등)를 받아서 고정 크기 리스트로 감싸 반환한다.
- 반환되는 `List`는 `java.util.Arrays$ArrayList`로, `add`/`remove`는 지원하지 않지만 `set`(원소 교체)은 가능하다.
- 원본 배열과 같은 메모리를 공유한다.

```java
List<Integer> list = Arrays.asList(1, 2, 3);
list.set(0, 9);   // OK
list.add(4);      // UnsupportedOperationException
```

## List.of() / Set.of() / Map.of() (Java 9+)

- 진짜 **불변 컬렉션(Immutable Collection)**을 생성한다.
- `add`, `remove`, `set` 모두 `UnsupportedOperationException`.
- `null` 원소 허용 안 함 (`NullPointerException`).

```java
List<Integer> list = List.of(1, 2, 3);
Set<String> set = Set.of("a", "b", "c");
Map<String, Integer> map = Map.of("a", 1, "b", 2);
```

## 비교

| 메서드 | 가변성 | null 허용 | 원본과 공유 |
| ---- | ---- | ---- | ---- |
| `Arrays.asList()` | 크기 고정, 원소 교체 가능 | O | O (배열과 공유) |
| `List.of()` | 완전 불변 | X | X |
| `new ArrayList<>(Arrays.asList(...))` | 완전 가변 | O | X |

## 사용 시점

- 진짜 불변이 필요하면: `List.of()`
- 크기는 고정하되 원소 교체가 필요하면: `Arrays.asList()`
- 만들고 나서 자유롭게 수정해야 하면: `new ArrayList<>(...)`로 한 번 더 감싸기
