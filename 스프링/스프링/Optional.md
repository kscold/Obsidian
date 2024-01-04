- Optional을 사용하는 이유는 `null`을 직접 다루는 것보다 안전하게 처리하기 위함이다.
- 자바에서 null 참조시 NullPointerException을 방지해주는 [[클래스(Class)]]를 말한다.  


###  예시 - 들어올 데이터 가 null일 때
- Optional.[[ofNullable()]]이 아닌 Optional.[[of()]] [[메서드(Method)]]를 이용하면 NullPointerException이 발생한다. Optional.of()는 들어올 데이터가 절대 null이 아닌 경우에만 사용하는데 null을 넣었기 때문이다. 

### 형식
- Optional<객체> [[findById()]] (Long id)  | findByName(String name)

- 이런 Optional 형태의 [[메서드(Method)]]들은 주어진 id나 name에 해당하는 객체를 찾아 반환한다.
- 만약 해당 ID의 멤버가 존재하지 않으면, 비어있는 Optional [[객체(Object)]]가 반환된다.