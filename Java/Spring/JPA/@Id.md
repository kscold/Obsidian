- `@Id`는 [[JPA(Java Persistence API)]] [[엔티티(entity)]]에서 **기본 키(Primary Key)** 필드를 지정하는 [[어노테이션(Annotation)]]이다.
- JPA는 `@Id` 필드를 통해 엔티티를 식별하고 영속성 컨텍스트를 관리한다.
- 반드시 `@GeneratedValue`와 함께 사용해 PK 생성 전략을 지정한다.

## @GeneratedValue 전략

| 전략 | 설명 | DB |
| ---- | ---- | ---- |
| `IDENTITY` | DB의 AUTO_INCREMENT에 위임 | MySQL, MariaDB, PostgreSQL SERIAL |
| `SEQUENCE` | DB 시퀀스 객체 사용 | PostgreSQL, Oracle |
| `TABLE` | 키 생성 테이블 사용 (성능 낮음, 범용) | 모든 DB |
| `AUTO` | DB 방언에 따라 위 중 자동 선택 | 기본값 |
| `UUID` | UUID 자동 생성 (JPA 3.1 / Spring Boot 3+) | 모든 DB |

## 사용 예시

```java
@Entity
public class Post {

    // MySQL/MariaDB — AUTO_INCREMENT
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
}
```

```java
@Entity
public class Post {

    // PostgreSQL — Sequence 전략
    @Id
    @GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "post_seq")
    @SequenceGenerator(name = "post_seq", sequenceName = "post_id_seq", allocationSize = 50)
    private Long id;
}
```

```java
@Entity
public class Post {

    // UUID (분산 환경, PK 노출 방지)
    @Id
    @GeneratedValue(strategy = GenerationType.UUID)
    private UUID id;
}
```

## Long vs String(UUID) 선택

| 항목 | `Long` (자동증가) | `UUID` |
| ---- | ---- | ---- |
| 성능 | 빠름 (정수 비교) | 느림 (16 byte 비교) |
| 노출 위험 | ID 순서 노출 (e.g. /posts/1) | 없음 |
| 분산 환경 | 충돌 위험 있음 | 충돌 없음 |
| 디버깅 | 쉬움 | 어려움 |

- MongoDB의 경우 `@Id`와 함께 `String` 타입 ObjectId를 사용한다.

## 관련

- [[JPA(Java Persistence API)]]
- [[엔티티(entity)]]
- [[기본 키(Primary Key)]]
- [[@Entity]]
- [[시퀀스(Sequence)]]
