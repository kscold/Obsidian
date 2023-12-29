- findById()는 [[JPA(Java Persistence API)]]의 [[CrudRepository]]가 제공하는 [[메서드(Method)]]이다.

- 특정 [[엔티티(entity)]]의 id 값을 기준으로 데이터를 찾아 [[Optional]] 타입으로 반환한다.


### 반환형 수정
- 실제 반환형이 Optional 타입이기 때문에


```java
Article articleEntity = articleRepository.findAllById(id);
// 기본 코드(오류)

Optinal<Article> articleEntity = articleRepository.findAllById(id);
// 옵셔널로 수정한 코드

Article articleEntity = articleRepository.findAllById(id).orElse(null);
// .orElse() 메소드로 수정한 코드
```