- findById()는 [[JPA(Java Persistence API)]]의 [[CrudRepository]]가 제공하는 [[메서드(Method)]]이다.
- 이 [[메서드(Method)]]는 주어진 findById(`id`)에 해당하는 [[엔티티(entity)]]를 찾는다.

- 반환 타입은 [[Optional]]`<T>`이며, `T`는 엔터티의 형식이다.
- 만약 해당 `id`에 대한 엔터티가 존재하면 해당 엔터티를 감싼 `Optional` 객체를 반환하고, 그렇지 않으면 비어있는 `Optional`을 반환한다.
- 특정 [[엔티티(entity)]]의 id 값을 기준으로 데이터를 찾아 [[Optional]] 타입으로 반환한다.
- 매개변수인 id가 null이면 `IllegalArgumentException` 오류가 난다.

- 특별히 조회나 데이터가 없는 경우도 처리해야 하므로 [[orElse()]] 메서드로 null이 반환되도록 한다.

