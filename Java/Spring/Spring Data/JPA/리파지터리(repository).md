- [[JPA(Java Persistence API)]]는 DB와의 소통을, 보다 쉽게 하고, 그 핵심이 되는 [[인터페이스(Interface)]]를 리파지터리(repository)라 한다.

- [[엔티티(Entity)]]가 DB 속 [[테이블(Table)]]에 저장 및 관리될 수 있게 하는 인터페이스이다.
- [[@Repository]] [[어노테이션(Annotation)]]으로 리파지터리를 정의한다.

![[Pasted image 20240106022633.png]]

## [[리파지터리(Repository)]]의 [[인터페이스(Interface)]] 구조

- 계층도의 밑으로 내려갈수록 low level 모듈이며 기능 구현이 많다.
### Repository 

- 최상위 [[리파지터리(Repository)]] [[인터페이스(Interface)]]이다.

### [[CrudRepository]] 및 ListCrudRepository

- [[엔티티(Entity)]]의 [[CRUD]] 기능을 제공한다.
### PagingAndSortingRepository 및 ListPagingAndSortingRepository

- [[엔티티(Entity)]]의 페이징 및 sorting 관련 기능들 제공한다.
### [[JpaRepository]]

- [[JPA(Java Persistence API)]] 관련 특화된 기능을 추가로 제공한다. 
- flushing, 배치성 작업 등을 제공한다.
- 추가적으로 CrudRepository와 PagingAndSortingRepository의 기능들을 제공한다.

- 단순하게 CRUD 작업만 한다면 [[CrudRepository]]를, 그 외에 페이징, sorting, jpa 특화 기능들을 사용하길 원한다면 [[JpaRepository]]를 사용하면 된다.

![[Pasted image 20240215001140.png]]


## 스프링 데이터 JPA 적용 후 클래스 다이어그램

- [[JpaRepository]]에서 엄청 많은 기능을 제공한다. 
- [[인터페이스(Interface)]] 상속 받아서 사용하면 된다.
- 기본적으로 [[CRUD]]를 구현하지 않아도 된다. 
- 인터페이스 호출해서 쓸 수 있다.
- 스프링 로딩 시점에 MemberRepository와 ItemRepository의 구현체를 만든다.

![](https://github.com/namjunemy/TIL/blob/master/Jpa/tacademy/img/15_jpa_repo_diagram.png?raw=true)

### 공통 인터페이스 기능

- JpaRepository 인터페이스는 공통 CRUD 제공한다.
- 제네릭은 <[[엔티티(Entity)]], [[식별자(Identifier)]]>로 설정한다.
- 스프링에 스프링 데이터 프로젝트와 스프링 데이터 JPA프로젝트가 따로 존재한다.
- 스프링 데이터 프로젝트에 공통적인 기능을 가지고 있고, JPA 특화된 기능을 스프링 데이터 JPA 프로젝트에서 가지고 있다.


![[Pasted image 20240229150251.png]]
### 쿼리 메서드 기능

- 위 이미지 스프링 데이터 [[JPA(Java Persistence API)]] 적용 전의 예시 코드를 보면 MemberRepository에 findByUsername() 메서드가 있다.

- JpaRepository만으로 이걸 어떻게 처리하나? 
- 이를 위해 쿼리 메서드라는 엄청난 기능이 존재한다.

- [[메서드(Method)]] 이름으로 쿼리를 생성한다.
- @Query [[어노테이션(Annotation)]]으로 [[쿼리(query)]]를 직접 정의할 수 있다.

### 메서드 이름으로 쿼리 생성

- 메서드 이름만으로 [[JPQL(Java Persitence Query Language)]] 쿼리를 생성한다.
- 선언된 메서드에 대해서는 애플리케이션 로딩 시점에 쿼리를 다 만들어 버린다.

- 그래서 에러를 사전에 잡을 수 있다.

```java
public interface MemberRepository extends JpaRepository<Member, Long> {  
	List<Member> findByName(String username);  
}

List<Member> member = memberRepository.findByName("hello")

// 실행된 SQL  
SELECT * FROM MEMBER M WHERE M.NAME = "hello"
```

### 이름으로 검색 + 정렬

- 자주 쓰는 기능 정렬을 위해 Sort를 넣어줄 수 있다.
- 스프링 데이터에서 만들어 놨다.

```java
public interface MemberRepository extends JpaRepository<Member, Long> {  
	List<Member> findByName(String username, Sort sort);  
}

// 실행된 SQL  
SELECT * FROM MEMBER M WHERE M.NAME = "hello" ORDER BY AGE DESC
```

### 이름으로 검색 + 정렬 + 페이징

- 추가로 페이징을 위한 기능도 만들어 놨다.
- 실제로 사용할땐 꼭 [[DTO(Data Transfer Object)]]로 변환해서 뿌린다.

- Page객체의 content에 데이터가 나오고, Pageable, sort, first, empty등의 정보를 얻을 수 있다.

```java
public interface MemberRepository extends JpaRepository<Member, Long> {  
	Page<Member> findByName(String username, Pageable pageable);  
}

// 예를 들어 이런식으로 사용할 수 있다.  
@GetMapping("/hello")  
public Page<Member> member() {  
	PageRequest request = PageRequest.of(1, 10);  
	return repository.findByName("hello1", request);  
}
```

```java
Pageable page = new PageRequest(1, 20, new Sort...);  
Page<Member> result = memberRepository.findByName("hello", page);  
  ​  
// 전체 수  
int total = result.getTotalElements();  
  ​  
// 데이터  
List<Member> members = result.getContent();  
```    

- 전체 페이지 수, 다음 페이지 및 페이징을 위한 API 다 구현되어 있다.

```sql
-- 실행된 SQL 2가지   

-- 데이터 조회
SELECT *  
	FROM  
	( SELECT ROW_.*, ROWNUM ROWNUM_  
	    FROM  
		    ( SELECT M.*  
			    FROM MEMBER M WHERE M.NAME = 'hello'  
			    OEDER BY M.
			) ROW_  
		WHERE ROWNUM <= ?  
	)
	WHERE ROWNUM_>?

-- 전체 수 조회
SELECT COUNT(1) FROM MEMBER M WEHRE M.NAME = 'hello'
```

### @Query, [[JPQL(Java Persitence Query Language)]] 정의

- @Query 어노테이션을 사용해서 직접 JPQL을 지정할 수 있다.
- 마찬가지로 Named 쿼리도 애플리케이션 로딩 시점에 파싱을 하므로, 이로 인해 런타임 에러를 내지 않을 수 있다.

```java
public interface MemberRepository extends JpaRepository<Member, Long> {  
          
	@Query("select m from Member m where m.username = ?1")  
	Member findByUsername(String username, Pageable pageable);  
}
```

### Web [[Paging]]과 정렬 기능

- 컨트롤러에서 페이징 처리 객체를 바로 인젝션 받을 수 있다.

#### page

- 현재 페이지이다.
####  size

- 한 페이지에 노출할 데이터 건수이다.

#### sort

- 정렬 조건이다.
- `/members?page=0&size=20&sort=name,desc`
```java
@RequestMapping(value = "/members", method = RequestMethod.GET)  
String list(Pageable pageable, Model mobel) {}
```

## [[QueryDSL]]

- [[SQL]], [[JPQL(Java Persitence Query Language)]]을 코드로 작성할 수 있도록 도와주는 빌더 API이다.
- JPA 크리테이라에 비해서 편리하고 실용적이다.
- 오픈소스이다.

### SQL, JPQL의 문제점

- SQL, JPQL은 문자열이다. 
- Type-check가 불가능하다.
- 잘 해봐야 애플리케이션 로딩 시점에 알 수 있다. 
- 컴파일 시점에 알 수 있는 방법이 없다. 
- 자바와 문자열의 한계이다.

- 해당 로직 실행 전까지 작동여부 확인을 할 수 없다.    
- 해당 쿼리 실행 시점에 오류를 발견한다.

```sql
SELECT * FROM MEMBERRR WHERE MEMBER_ID = '100'
```

### QueryDSL 장점

- 문자가 아닌 코드로 작성한다.
- 컴파일 시점에 문법 오류를 발견할 수 있다.
- 코드 자동완성(IDE 도움)이 된다.
- 단순하고 쉽거,  코드 형태가 [[JPQL(Java Persitence Query Language)]]과 거의 비슷하다   .
- 동적 쿼리를 생성할 수 있다.
