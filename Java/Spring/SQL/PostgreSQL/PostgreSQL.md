- PostgreSQL(Postgres)은 표준 SQL 준수도가 높고 기능이 풍부한 오픈소스 객체-관계형 데이터베이스이다.
- JSONB, 배열 타입, 전문 검색, 파티셔닝 등 고급 기능이 강점이다.
- 최근 Spring Boot + JPA 조합에서 MySQL 대신 PostgreSQL을 선택하는 프로젝트가 늘고 있다.

## MySQL vs PostgreSQL 주요 차이

| 항목 | MySQL / MariaDB | PostgreSQL |
| ---- | ---- | ---- |
| 라이선스 | GPL | PostgreSQL License (BSD-style) |
| JSON 지원 | JSON 타입 (텍스트 기반) | `JSONB` (바이너리, 인덱스 가능) |
| 기본 격리 수준 | REPEATABLE READ | READ COMMITTED |
| AUTO INCREMENT | `AUTO_INCREMENT` | `SERIAL` / `GENERATED ALWAYS AS IDENTITY` |
| 대소문자 | 기본 대소문자 무시 | 대소문자 구분 |
| 논리 복제 | 제한적 | 내장 지원 |
| 확장성 | - | Extension (uuid-ossp, pg_trgm 등) |

## Spring Boot 연동

```yaml
# application.yml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/mydb
    username: postgres
    password: password
    driver-class-name: org.postgresql.Driver
  jpa:
    database-platform: org.hibernate.dialect.PostgreSQLDialect
    hibernate:
      ddl-auto: validate
    show-sql: false
```

```gradle
// build.gradle
runtimeOnly 'org.postgresql:postgresql'
```

## AUTO INCREMENT 방식

```sql
-- SERIAL (구 방식, 내부적으로 SEQUENCE 생성)
CREATE TABLE posts (
    id SERIAL PRIMARY KEY,
    title VARCHAR(200)
);

-- GENERATED ALWAYS AS IDENTITY (표준 SQL, PostgreSQL 10+)
CREATE TABLE posts (
    id INTEGER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    title VARCHAR(200)
);
```

- JPA에서 `@GeneratedValue(strategy = GenerationType.IDENTITY)`를 사용하면 자동으로 SERIAL에 매핑된다.

## 주요 특징

### JSONB

- JSON을 바이너리 형태로 저장 → **인덱스 생성 가능**, 빠른 조회.
- `@Column(columnDefinition = "jsonb")`으로 JPA 매핑.
- 자세한 내용: [[JSONB]]

### UUID 사용

```sql
-- uuid-ossp 확장 설치
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

-- UUID 기본값
id UUID DEFAULT uuid_generate_v4() PRIMARY KEY
```

```java
@Id
@GeneratedValue(strategy = GenerationType.UUID)
private UUID id;
```

### 파티셔닝

- 대용량 테이블을 조건 기반으로 물리적으로 분할.
- `RANGE`, `LIST`, `HASH` 파티셔닝 지원.

## 관련

- [[관계형 데이터베이스(Relational DataBase)]]
- [[MySQL(MariaDB)]]
- [[JSONB]]
- [[트랜잭션(Transaction)]]
- [[인덱스(Index)]]
- [[JPA(Java Persistence API)]]
