- 이 [[메서드(Method)]]는 주어진 `id`들에 해당하는 모든 [[엔티티(entity)]]를 찾는다.

- 반환 타입은 `Iterable<T>`이다.
- 여러 `id`에 대한 결과를 반환할 수 있으므로 [[Iterable]]로 감싸져 있다.
- 반환되는 요소들의 순서는 보장되지 않는다.
- 어떤 id도 발견되지 않으면, 어떤 엔티티도 반환되지 않는다.

- 파라미터의 ids나 ids중 하나라도 null이어서는 안 된다.
- 따라서 ids나 ids중 하나라도 null이면 `IllegalArgumentException` 오류가 난다.

- 단일 `id`를 사용하는 경우에도 `findAllById(id)`를 사용할 수 있지만, 반환형이 `Iterable`이므로 주의가 필요하다.