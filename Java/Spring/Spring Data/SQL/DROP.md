- `DROP`은 SQL DDL(Data Definition Language)에서 **데이터베이스 객체(테이블, 뷰, 인덱스 등)를 완전히 삭제**하는 명령어이다.
- 테이블 구조와 모든 데이터, 인덱스, 트리거가 함께 삭제된다.
- **되돌릴 수 없으므로** 운영 환경에서는 반드시 백업 후 실행해야 한다.

## 기본 문법

```sql
-- 테이블 삭제
DROP TABLE 테이블명;
DROP TABLE IF EXISTS 테이블명;   -- 존재할 때만 삭제 (에러 방지)

-- 데이터베이스 삭제
DROP DATABASE 데이터베이스명;

-- 뷰 삭제
DROP VIEW 뷰명;

-- 인덱스 삭제
DROP INDEX 인덱스명 ON 테이블명;        -- MySQL
DROP INDEX 인덱스명;                    -- PostgreSQL
```

## DROP vs TRUNCATE vs DELETE 비교

| 명령어 | 구조 삭제 | 데이터 삭제 | 롤백 가능 | 속도 |
| ---- | ---- | ---- | ---- | ---- |
| `DROP TABLE` | O | O | 불가 (DDL) | 빠름 |
| `TRUNCATE TABLE` | X | O (전체) | 불가 (DDL) | 빠름 |
| `DELETE FROM` | X | O (조건 가능) | 가능 (DML) | 느림 |

```sql
-- 테이블 구조는 유지하고 데이터만 전체 삭제
TRUNCATE TABLE posts;

-- 조건부 행 삭제 (트랜잭션 가능)
DELETE FROM posts WHERE status = 'draft';
```

## 외래 키 제약 처리

```sql
-- 외래 키가 있는 테이블 삭제 시 에러 발생 → 제약 해제 후 삭제
SET FOREIGN_KEY_CHECKS = 0;  -- MySQL
DROP TABLE child_table;
DROP TABLE parent_table;
SET FOREIGN_KEY_CHECKS = 1;

-- CASCADE 옵션으로 참조하는 테이블도 같이 삭제 (PostgreSQL)
DROP TABLE parent_table CASCADE;
```

## JPA ddl-auto와의 관계

- JPA `spring.jpa.hibernate.ddl-auto=create` 설정 시 → 시작 시 기존 테이블 DROP 후 재생성.
- **운영 환경에서 절대 `create` 사용 금지** → `validate` 또는 Flyway/Liquibase 마이그레이션 도구 사용.

## 관련

- [[SQL]]
- [[CREATE]]
- [[DELETE]]
- [[UPDATE]]
- [[MySQL(MariaDB)]]
- [[JPA(Java Persistence API)]]
