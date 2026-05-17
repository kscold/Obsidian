- 영속성 컨텍스트(Persistence Context)는 **[[JPA(Java Persistence API)]]가 [[엔티티(entity)]] [[객체(Object)]]를 관리하는 논리적인 공간**이다.
- `EntityManager`가 영속성 컨텍스트와 1:1로 연결되어 [[엔티티(entity)]]의 [[생명 주기(Life Cycle)]]를 관리한다.
- [[스프링 부트 한글 깨짐]] + [[JPA(Java Persistence API)]] 환경에서는 [[@Transactional]] 범위 내에서 자동으로 동작한다.

## [[엔티티(entity)]] 생명주기

```mermaid
flowchart LR
    new["비영속(Transient)<br/>new Post()"] -->|em.persist()| managed["영속(Managed)<br/>컨텍스트가 관리"]
    managed -->|트랜잭션 커밋| db[(DB)]
    managed -->|em.detach()| detached["준영속(Detached)<br/>컨텍스트에서 분리"]
    managed -->|em.remove()| removed["삭제(Removed)<br/>DB에서 제거 예정"]
    detached -->|em.merge()| managed
```

| 상태 | 설명 | DB 반영 |
| ---- | ---- | ---- |
| 비영속(Transient) | `new`로 생성만 한 상태 | X |
| 영속(Managed) | 컨텍스트가 관리하는 상태 | [[트랜잭션(Transaction)]] 커밋 시 |
| 준영속(Detached) | 컨텍스트에서 분리된 상태 | X |
| 삭제(Removed) | 삭제 예약 상태 | 트랜잭션 커밋 시 |

## 핵심 기능 4가지

### 1. 1차 캐시 (First Level Cache)

- 영속성 컨텍스트 내부에 Map 형태로 엔티티를 보관한다.
- 같은 트랜잭션 안에서 같은 PK로 조회하면 DB를 거치지 않고 캐시에서 반환한다.

```java
Post post1 = postRepository.findById(1L).get();  // [[데이터베이스(DataBase)]] 조회 → 캐시 저장
Post post2 = postRepository.findById(1L).get();  // DB 조회 없이 캐시에서 반환

System.out.println(post1 == post2);  // true — 동일한 객체 (동일성 보장)
```

### 2. 변경 감지 (Dirty Checking)

- [[트랜잭션(Transaction)]] 커밋 시점에 **스냅샷(최초 조회 시 상태)**과 현재 객체를 비교해서 변경된 [[필드(Field)]]만 [[UPDATE]]한다.
- `[[save()]]`를 명시적으로 호출하지 않아도 [[필드(Field)]] 값을 바꾸면 [[데이터베이스(DataBase)]]에 반영된다.

```java
@Transactional
public void updateTitle(Long id, String newTitle) {
    Post post = postRepository.findById(id).get();  // 영속 상태
    post.setTitle(newTitle);                          // 변경 감지 등록
    // save() 없이도 트랜잭션 종료 시 UPDATE 쿼리 자동 실행
}
```

### 3. 쓰기 지연 (Write-Behind)

- [[SQL]]을 즉시 DB에 보내지 않고 **쓰기 지연 저장소**에 모아두었다가 트랜잭션 커밋 직전에 한꺼번에 실행한다.
- 여러 [[INSERT]]/[[UPDATE]]를 모아서 배치로 실행하면 DB I/O가 줄어든다.

```java
@Transactional
public void saveMultiple() {
    em.persist(new Post("제목1"));  // INSERT 쿼리 아직 안 나감
    em.persist(new Post("제목2"));  // INSERT 쿼리 아직 안 나감
    // 여기까지 DB 조회 없음
    em.flush();  // 또는 트랜잭션 커밋 → INSERT 2개 한꺼번에 전송
}
```

### 4. 동일성 보장 (Identity)

- 같은 트랜잭션 안에서 같은 PK로 조회한 엔티티는 `==` 비교가 `true`가 된다.
- Java 컬렉션에서 동일 객체를 꺼내는 것과 같은 개념.

## flush() vs commit()

| 동작 | 설명 |
| ---- | ---- |
| `flush()` | 쓰기 지연 저장소의 SQL을 DB에 전송. 트랜잭션은 아직 유지. |
| 커밋(commit) | flush() 후 트랜잭션 종료. DB에 영구 반영. |
| 롤백(rollback) | 1차 캐시와 DB를 모두 원상복구. |

## OSIV (Open Session In View)

- Spring Boot에서 기본값 `spring.jpa.open-in-view=true`.
- [[HTTP]] 요청 시작부터 응답 완료까지 영속성 컨텍스트를 열어둔다.
- 장점: [[뷰(View)]]/[[컨트롤러(Controller)]]에서도 [[지연 로딩(Lazy Loading)]] 가능.
- **단점: [[DBCP(Database Connection Pool)]] 커넥션을 요청 전체 시간 동안 점유 → 트래픽 많으면 커넥션 부족**.
- 실무 권장: `spring.jpa.open-in-view=false` + [[서비스(Service)]] 계층에서 [[DTO(Data Transfer Object)]]로 변환.

```yaml
# application.yml
spring:
  jpa:
    open-in-view: false   # 실무 권장 설정
```

## [[@Transactional]]과의 관계

```java
@Transactional  // 트랜잭션 시작 = 영속성 컨텍스트 시작
public PostDto findPost(Long id) {
    Post post = postRepository.findById(id).get();  // 영속 상태
    return new PostDto(post);   // DTO 변환 후 반환
}   // 트랜잭션 종료 = 영속성 컨텍스트 종료 = 영속 → 준영속
```

- 메서드가 끝나면 영속성 컨텍스트가 닫히고 엔티티는 **준영속 상태**가 된다.
- 준영속 상태에서 [[지연 로딩(Lazy Loading)]]을 시도하면 `LazyInitializationException` 발생.

## 관련

- [[JPA(Java Persistence API)]]
- [[@Transactional]]
- [[지연 로딩(Lazy Loading)]]
- [[즉시 로딩(Eager Loading)]]
- [[N+1 문제(N+1 Problem)]]
- [[낙관적 락(Optimistic Lock)]]
- [[비관적 락(Pessimistic Lock)]]
