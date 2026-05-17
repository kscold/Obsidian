- 데이터를 저장 하려면, DB에 넘겨야 하는데 자바와 [[SQL]]로 사용하는 언어의 차이 때문에 직접 데이터를 넘기기가 쉽지 않다.
- 따라서 이를 위한 라이브러리가 JPA(Java Persistence API)라고 부르며, 정리하면 서버와 DB 간의 소통에 관여하는 기술이다.

- 자바 표준 ORM(Object-Relational Mapping) 기술이다.
- ORM은 객체 지향 프로그래밍 언어와 [[관계형 데이터베이스(Relational DataBase)]] 사이의 데이터를 매핑하는 기술을 의미한다.

- JPA의 핵심 도구는 [[엔티티(Entity)]]와 [[리파지터리(Repository)]]이 있다.

- 자바 언어로 DB에 명령을 내리는 도구로, 데이터를 객체 지향적으로 관리할 수 있게 해준다.

- [[JDBC(Java DataBase Connectivity)]]와 JPA(Java Persistence API) 모두 [[데이터베이스(DataBase)]]와 상호작용하기 위한 기술이나, JPA는 개발자는 [[SQL]] 쿼리를 직접 작성하는 대신 JPA의 기능을 사용하여 데이터베이스 작업을 수행할 수 있다.


## JPA에서 컬렉션

- JPA는 자바에서 기본으로 제공하는 [[Collection]], [[List]], [[Set]], [[Map]] 컬렉션을 지원하고, 아래와 같은 상황에서 컬렉션을 사용할 수 있다.

- [[@OneToMany]], @ManyToMany를 사용해서 [[일대다(OnetoMany)]]나 [[다대다(ManyToMany)]] 관계를 매핑할 때 사용한다.
- @ElementCollection 을 사용해서 값 타입을 하나 이상 보관할 때 사용한다.

- [[Hibernate]]는 [[엔티티(Entity)]]를 영속상태로 만들 때 컬렉션 [[Spring/SQL/필드(Field)|필드(Field)]]를 하이버네이트에서 준비한 컬렉션으로 감싸서 사용 한다. 
- 이는 [[Hibernate]]가 컬렉션을 효율적으로 관기하기 위함이다.  
- 하이버네이트는 본 컬렉션을 감싸고 있는 내장 컬렉션을 생성한 뒤, 이 내장 컬렉션을 사용하도록 참조를 변경한다.


## JPA 반환 타입
### Page`<T>

- `Page<T>` 타입을 반환 타입으로 받게 된다면 offset과 totalPage 를 이용하여 서비스를 제공할 수 있게된다.
- `Page<T>` 는 일반적인 게시판 형태의 페이징에서 사용된다.

![](https://blog.kakaocdn.net/dn/mtn8w/btq2gEQa7Hq/bC5skfEAFRJ3F5rBLfDpW1/img.png)

- 여기서 중요한 정보는 총 페이지 수이다.
- 그 정보를 포함하여 반환한다.

- Page`<T>` 타입은 count [[쿼리(query)]]를 포함하는 페이징으로 카운트 쿼리가 자동으로 생성되어 함께 나간다.

### Slice`<T>` 타입

Slice`<T>` 타입을 반환 타입으로 받게 된다면 더보기 형태의 페이징([[Paging]])에서 사용된다.

![](https://blog.kakaocdn.net/dn/6RNYg/btq2ieCHGiH/EqncoN0p4eq9LHnhEfvTB1/img.png)

- 응답을 살펴보면 다음과 같다.

```json
{
  "content": [
    { "id": 13, "username": "User 12", "address": "Korea", "age": 12 },
    { "id": 14, "username": "User 13", "address": "Korea", "age": 13 },
    { "id": 15, "username": "User 14", "address": "Korea", "age": 14 },
    { "id": 16, "username": "User 15", "address": "Korea", "age": 15 }
  ],
  "pageable": {
    "sort": { "sorted": false, "unsorted": true, "empty": true },
    "pageNumber": 3,
    "pageSize": 4,
    "offset": 12,
    "paged": true,
    "unpaged": false
  },
  "number": 3,
  "numberOfElements": 4,
  "first": false,
  "last": false,
  "size": 4,
  "sort": { "sorted": false, "unsorted": true, "empty": true },
  "empty": false
}
```

- Page`<T>` 타입의 반환에 없는 것들이 존재한다.
  
- number과 numberOfElements 그리고 Page`<T>` 에 존재하던 totalPages, totalElements 가 없어졌다.

- Slice`<T>` 타입은 추가 count 쿼리 없이 다음 페이지 확인 가능하다. 
- 내부적으로 limit + 1 조회를 해서 totalCount 쿼리가 나가지 않아서 성능상 조금 이점을 볼 수도 있다.

### List`<T>` 타입

```java
@GetMapping("/users")
public List<User> getAllUsers(Pageable pageable) {
	return userRepository.findAll(pageable);
}
```

- [[List]] 반환 타입은 가장 기본적인 방법으로 count [[쿼리(query)]] 없이 결과만 반환한다.

## Page vs Slice

```java
Page<User> findByLastname(String lastName, Pageable pageable);

Slice<User> findByLastname(String lastName, Pageable pageable);

List<User> findByLastname(String lastName, Pageable pageable);
```

- JPA에서는 반환 값으로 List, Slice, Page로 다양하게 제공하고 있다. 
- Page 구현체의 경우에는 전체 Page의 크기를 알아야 하므로, 필요한 Page의 요청과 함께, 전체 페이지 수를 계산하는 count [[쿼리(query)]]가 별도로 실행된다. 

- Slice 구현체의 경우에는 전후의 Slice가 존재하는지 여부에 대한 정보를 가지고 있다.

- spring.jpa.show-sql=true 옵션을 설정 파일에 추가하면 Page를 반환하는 [[메서드(Method)]] 실행 시, 실제로 count 명령어가 실행되는 것을 확인 할 수 있다.

- Slice는 [[Iterable]]을 구현하고 [[List]]와 페이지처리에 쓸만한 다양한 [[멤버 변수(Member Variable)]], [[메서드(Method)]]들을 명시한 [[인터페이스(Interface)]]이다.

- Page는 Slice를 확장한 [[인터페이스(Interface)]]로 사용시에는 JPA 내부에서 페이징 처리를 하려하는 데이터들의 총 갯수(total count) 쿼리를 하나 더 날리게되며, 이를 활용하는 getTotalPages(), getTotalElements() 메서드 등을 사용할 수 있다.

- 따라서 페이징 박스를 렌더링하는 정책에 따라 Page나 Slice중 하나를 골라 사용하면 된다.


- Spring JPA에서는 페이징 기능을 제공한다.
- JPA Repository를 사용하면서 매개변수에 Pageable만 추가하면 끝이다.

- 그 이유는 이미 [[JpaRepository]]가 PagingAndSortingRepository를 구현하고 있기 때문이었다.
- Pageable 구현체는 PageRequest 를 사용하면 되고, 페이지 번호와 페이지 사이즈를 넣어주면 된다.
- Page, Slice 혹은 List를 반환형으로 가지며, Page는 전체 페이지 수와 전체 데이터수를 가진다.
- Slice는 count는 알지 못하고 단지 더보기 기능을 위해 limit + 1을 조회할 뿐이다.
- List는 어떠한 count도 알지 못한다.