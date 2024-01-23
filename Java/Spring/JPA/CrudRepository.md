- CrudRepository는 [[JPA(Java Persistence API)]]에서 제공하는 [[인터페이스(Interface)]]로 이를 상속해 [[엔티티(entity)]]를 관리(생성, 조회, 수정, 삭제)할 수 있다.
- [[클래스(Class)]]를 선언 후 [[extends]]를 통해 CrudRepository선택하여 가져올 수 있다.
- [[@Repository]] [[어노테이션(Annotation)]]를 사용하여 좀 더 명시적으로 표현할 수 있다.
- [[리파지터리(repository)]]에서 설정한다.

## 문법

- CrudRepository에 홑화살괄호(<>)를 붙이고 그 안에 다음과 같이 2개의 제네릭 요소를 받는다.

1. T
	 - 관리 대상 [[엔티티(entity)]]의 [[클래스(Class)]] 타입이다. 
	 - 책 예시는 Article이다.

3. ID
	- 관리 대상 [[엔티티(entity)]]의 대푯값 타입니다. 
	- Article.java 파일에 가 보면 id를 대푯값으로 지정했다.
	- id의 타입은 Long이므로 Long을 입력한다.

## 예시

```java
@Repository
public interface ArticleRepository extends CrudRepository<Article, Long> { // CrudRepository를 상속
    @Override  
    ArrayList<Article> findAll(); // Iterable -> ArrayList 수정(다운 캐스팅)
}
```

### CrudRepository로 상속된 [[인터페이스(Interface)]]의 [[메서드(Method)]]

long count()
    - 가용 entity의 수를 반환함
    - 리포지토리의 데이터 개수 확인

#### Delete

- `void delete(T entity)`
    - 주어진 entity를 삭제함
    - entity가 null이면 `IllegalArgumentException`을 던짐
- `void deleteById(ID id)`
    - 주어진 id를 가진 엔티티를 삭제함
    - id가 null이면 `IllegalArgumentException`을 던짐
- `void deleteAll()`
    - 리포지토리에 의해 관리되는 모든 엔티티를 삭제함
- `void deleteAll(Iterable<? extends T> entities)`
    - 주어진 엔티티들을 삭제함
    - entities 및 entities들 중 하나라도 null이어서는 안 됨
    - entities 및 entities들 중 하나라도 null이면 `IllegalArgumentException`을 던짐

#### Read (조회)

- [[Optional]]`<T>` [[findById()]], 매개변수(`id`)
	- 주어진 id를 가진 [[엔티티(entity)]]가 존재하는지 반환한다.
	- 조회된 엔티티 객체를 Optional로 감싸서 반환한다.
	- id가 null이면 `IllegalArgumentException`을 던진다.

- `boolean existsById(ID id)`
    - 주어진 id를 가진 엔티티가 존재하는지 반환함
    - 반환 : 존재하면 true, 아니면 false
    - id가 null이면 `IllegalArgumentException`을 던짐
    
- `Iterable<T>` [[findAll()]]
	- 리포지토리의 모든 인스턴스(엔티티)를 반환함 (`Iterable<T>`타입으로)
    
- `Iterable<T>` [[findAllById()]], 매개변수(`Iterable<ID> ids`)
    - 리포지토리에서 T타입이면서 주어진 ID들에 해당하는 모든 인스턴스를 T타입으로 반환함
    - 리턴되는 요소들의 순서는 보장되지 않음.
    - 어떤 id도 발견되지 않으면, 어떤 엔티티도 반환되지 않음.
    - 파라미터 : ids나 ids중 하나라도 null이어서는 안 됨.
    - ids나 ids중 하나라도 null이면 `IllegalArgumentException`을 던짐

#### Create (저장)

- `<S extends T> S save(S entity)`
    - 주어진 엔티티를 저장함
    - 저장된 엔티티를 반환함
    - 주어진 엔티티가 null이면 `IllegalArgumentException`을 던짐
    
- `<S extends T> Iterable<S> saveAll(Iterable<S> entities)`
    - 주어진 모든 엔티티들을 저장함
    - 저장된 엔티티들을 반환함
    - 주어진 엔티티들(또는 엔티티들 중 하나)가 null이면 `IllegalArgumentException`을 던짐

