- findById()는 [[JPA(Java Persistence API)]]의 [[CrudRepository]]가 제공하는 [[메서드(Method)]]이다.
- 이 [[메서드(Method)]]는 주어진 findById(`id`)에 해당하는 [[엔티티(Entity)]]를 찾는다.

- 반환 타입은 [[Optional]]`<T>`이며, `T`는 [[엔티티(Entity)]]를 [[Optional]]로 감싼 반환 타입의 형식이다.
- 만약 해당 `id`에 대한 [[엔티티(Entity)]]가 존재하면 해당 엔티티를 감싼 [[Optional]] [[객체(Object)]]를 반환하고, 그렇지 않으면 비어있는 [[Optional]]을 반환한다.

- 매개변수인 id가 null이면 `IllegalArgumentException` 오류가 난다.
- 특별히 조회나 데이터가 없는 경우도 처리해야 하므로 [[Optional.orElse()]] 메서드로 null이 반환되도록 한다.

