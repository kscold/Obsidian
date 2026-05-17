- CrudRepository는 [[JPA(Java Persistence API)]]에서 제공하는 [[인터페이스(Interface)]]로 이를 상속해 [[엔티티(Entity)]]를 관리(생성, 조회, 수정, 삭제)할 수 있다.

- [[클래스(Class)]]를 선언 후 [[extends]]를 통해 CrudRepository선택하여 가져올 수 있다.
- [[@Repository]] [[어노테이션(Annotation)]]를 사용하여 좀 더 명시적으로 표현할 수 있다.
- [[리파지터리(Repository)]]에서 설정한다.

- 우리가 정의하지 않은 기능을 인터페이스로 제공하면, 문제가 발생할 수 있다.
- 예를 들어, 삭제 메서드는 제공하지 않기를 바라지만 jpaRepository 는 삭제 메서드와 필요 없는 다른 메서드들에 대한 인터페이스도 제공한다.

- 해결법은 우리가 필요한 메서드만 정의한다면 시스템은 훨씬 간단명료해지고 [[인터페이스(Interface)]]로 제공하는 메서드 수는 훨씬 적어진다. 
- 필요한 메서드가 있다면, 만들면 된다.
- 정말 필요한 메서드만 선언해서 사용함에 따라, 메서드의 재사용성이 높아진다.
- 이는 쿼리 캐싱에 성공할 확률이 높아지는 효과를 가진다.

## 문법

- CrudRepository에 홑화살괄호(<>)를 붙이고 그 안에 다음과 같이 2개의 제네릭 요소를 받는다.

1. T
	 - 관리 대상 [[엔티티(Entity)]]의 [[클래스(Class)]] 타입이다. 
	 - 책 예시는 Article이다.

3. ID
	- 관리 대상 [[엔티티(Entity)]]의 대푯값 타입니다. 
	- Article.java 파일에 가 보면 id를 대푯값으로 지정했다.
	- id의 타입은 Long이므로 Long을 입력한다.

## 예시

- 밑의 코드는 [[리파지터리(Repository)]] [[클래스(Class)]]를 만들고 [[@Repository]] [[어노테이션(Annotation)]]을 붙여 CrudRepository를 [[상속(Inheritance)]]받는 예시이다.

```java
@Repository
public interface ArticleRepository extends CrudRepository<Article, Long> { // CrudRepository를 상속
    @Override  
    ArrayList<Article> findAll(); // Iterable -> ArrayList 수정(다운 캐스팅)
}
```


## CrudRepository [[메서드(Method)]]

### long count()

- 가용 entity의 수를 반환한다.
- 리포지토리의 데이터 개수 확인한다.
### Delete

#### void delete(T entity)    

- 주어진 entity를 삭제한다.
- entity가 null이면 IllegalArgumentException을 던진다.
#### void deleteById(ID id)

   - 주어진 id를 가진 엔티티를 삭제한다.
   - id가 null이면 IllegalArgumentException을 던진다.
#### void deleteAll()

- 리포지토리에 의해 관리되는 모든 엔티티를 삭제한다.

#### void deleteAll(Iterable`<? extends T>` entities)

   - 주어진 [[엔티티(Entity)]]들을 삭제한다.
   - entities 및 entities들 중 하나라도 null이어서는 안된다.
   - entities 및 entities들 중 하나라도 null이면 IllegalArgumentException을 던진다.

### Read (조회)

#### [[Optional]]`<T>` [[findById()]], 매개변수(`id`)

- 주어진 id를 가진 [[엔티티(Entity)]]가 존재하는지 반환한다.
- 조회된 엔티티 객체를 [[Optional]]로 감싸서 반환한다.
- id가 null이면 IllegalArgumentException을 던진다.

#### boolean existsById(ID id)

- 주어진 id를 가진 엔티티가 존재하는지 반환함
- 반환 : 존재하면 true, 아니면 false
- id가 null이면 `IllegalArgumentException`을 던짐
   
#### [[Iterable]]`<T>`[[findAll()]]

- [[리파지터리(Repository)]]의 모든 [[인스턴스(Instance)]]([[엔티티(Entity)]])를 반환한다. ([[Iterable]]`<T>`타입으로 반환한다.)
    
#### [[Iterable]]`<T>` [[findAllById()]], 매개변수([[Iterable]]`<ID> ids`)

- 리포지토리에서 T타입이면서 주어진 ID들에 해당하는 모든 인스턴스를 T타입으로 반환한다.
- 리턴되는 요소들의 순서는 보장되지 않는다.
- 어떤 id도 발견되지 않으면, 어떤 엔티티도 반환되지 않는다.
- 매개변수는 ids나 ids중 하나라도 null이어서는 안 된다.
- ids나 ids중 하나라도 null이면 IllegalArgumentException을 던진다.

### Create (저장)

#### `<S extends T> S` [[save()]], 매개변수(S entity)

- 매개변수로 주어진 [[엔티티(Entity)]]를 저장한다.
- 저장된 [[엔티티(Entity)]]를 반환한다.
- 매개변수로 주어진 엔티티가 null이면 IllegalArgumentException 오류를 반환한다.

#### `<S extends T>` [[Iterable]]`<S>` saveAll([[Iterable]]`<S>` entities)

- 주어진 모든 엔티티들을 저장한다.
- 저장된 엔티티들을 반환한다.
- 주어진 엔티티들(또는 엔티티들 중 하나)가 null이면 IllegalArgumentException을 던진다.

