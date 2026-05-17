- MySQL/MariaDB의 데이터 타입은 컬럼에 저장할 값의 종류와 크기를 정의한다.
- 적절한 타입 선택은 저장 공간 절약과 쿼리 성능에 직결된다.

## 숫자형

| 타입 | 크기 | 범위 (부호 있음) | 용도 |
| ---- | ---- | ---- | ---- |
| `TINYINT` | 1 byte | -128 ~ 127 | 불리언 대체, 상태 코드 |
| `SMALLINT` | 2 byte | -32,768 ~ 32,767 | |
| `INT` / `INTEGER` | 4 byte | -2,147,483,648 ~ 2,147,483,647 | 일반 ID, 카운터 |
| `BIGINT` | 8 byte | ±9,223,372,036,854,775,807 | 대용량 ID, epoch time |
| `DECIMAL(p, s)` | 가변 | 정밀도 보장 | 금액, 소수 계산 |
| `FLOAT` / `DOUBLE` | 4/8 byte | 부동소수점 | 부정확, 금액에 사용 금지 |

- JPA에서 `Long` → `BIGINT`, `Integer` → `INT`, `boolean` → `TINYINT(1)` 로 매핑된다.

## 문자형

| 타입 | 최대 크기 | 특징 | 용도 |
| ---- | ---- | ---- | ---- |
| `CHAR(n)` | 255자 | 고정 길이, 빠른 비교 | 국가코드, 해시값 |
| `VARCHAR(n)` | 65,535 byte | 가변 길이, 효율적 | 이름, 이메일, URL |
| `TEXT` | 65,535 byte | 인덱스 제한 있음 | 본문, 설명 |
| `MEDIUMTEXT` | 16 MB | | 긴 본문 |
| `LONGTEXT` | 4 GB | | 매우 긴 텍스트 |
| `JSON` | LONGTEXT 기반 | JSON 유효성 검사 | JSON 저장 (MySQL 5.7+) |

- `VARCHAR`는 n이 255 이하이면 1 byte, 256 이상이면 2 byte 길이 헤더를 사용한다.

## 날짜/시간형

| 타입 | 범위 | 특징 | 용도 |
| ---- | ---- | ---- | ---- |
| `DATE` | 1000-01-01 ~ 9999-12-31 | 날짜만 | 생일, 만료일 |
| `TIME` | -838:59:59 ~ 838:59:59 | 시간만 | |
| `DATETIME` | 1000-01-01 ~ 9999-12-31 | 날짜+시간, 타임존 저장 안 함 | createdAt |
| `TIMESTAMP` | 1970-01-01 ~ 2038-01-19 | UTC 저장, 조회 시 서버 타임존 변환 | 자동 갱신에 사용 |
| `YEAR` | 1901 ~ 2155 | | |

- JPA `LocalDateTime` → `DATETIME`, `Instant` → `DATETIME(6)` (마이크로초) 로 매핑.
- **운영 DB는 모든 시간을 UTC로 저장**하는 것이 권장된다.

## 이진형

| 타입 | 용도 |
| ---- | ---- |
| `BINARY(n)` / `VARBINARY(n)` | 고정/가변 이진 데이터 |
| `BLOB` / `LONGBLOB` | 파일, 이미지 (실제 파일은 S3 권장) |

## JPA 타입 매핑 요약

```java
@Entity
public class Post {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;                        // BIGINT AUTO_INCREMENT

    @Column(nullable = false, length = 200)
    private String title;                   // VARCHAR(200)

    @Column(columnDefinition = "TEXT")
    private String content;                 // TEXT

    @Column(precision = 10, scale = 2)
    private BigDecimal price;               // DECIMAL(10, 2)

    private LocalDateTime createdAt;        // DATETIME(6)
}
```

## 관련

- [[MySQL(MariaDB)]]
- [[JPA(Java Persistence API)]]
- [[관계형 데이터베이스(Relational DataBase)]]
