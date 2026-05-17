- `NOW()`는 MySQL/MariaDB에서 **현재 날짜와 시각을 반환**하는 함수이다.
- `DATETIME` 형식(`YYYY-MM-DD HH:MM:SS`)으로 반환하며, 서버의 타임존 기준이다.

## 날짜/시간 함수 전체 정리

| 함수 | 반환 타입 | 설명 |
| ---- | ---- | ---- |
| `NOW()` | DATETIME | 현재 날짜 + 시각 (`2024-05-17 14:30:00`) |
| `CURDATE()` / `CURRENT_DATE()` | DATE | 현재 날짜만 (`2024-05-17`) |
| `CURTIME()` / `CURRENT_TIME()` | TIME | 현재 시각만 (`14:30:00`) |
| `SYSDATE()` | DATETIME | 함수 실행 시점 시각 (NOW와 미묘한 차이) |
| `UTC_TIMESTAMP()` | DATETIME | UTC 기준 현재 시각 |

## 사용 예시

```sql
-- 현재 시각 조회
SELECT NOW();
-- 2024-05-17 14:30:00

-- 생성일을 현재 시각으로 삽입
INSERT INTO posts (title, created_at) VALUES ('제목', NOW());

-- 특정 기간 내 데이터 조회 (최근 7일)
SELECT * FROM posts
WHERE created_at >= NOW() - INTERVAL 7 DAY;

-- 날짜 차이 계산
SELECT DATEDIFF(NOW(), created_at) AS days_since_created
FROM posts;
```

## 날짜 형식 변환

```sql
-- 날짜 포맷 변환
SELECT DATE_FORMAT(NOW(), '%Y년 %m월 %d일');
-- 2024년 05월 17일

-- 날짜에서 특정 부분 추출
SELECT YEAR(NOW()), MONTH(NOW()), DAY(NOW());
SELECT HOUR(NOW()), MINUTE(NOW()), SECOND(NOW());
```

## 날짜 연산

```sql
-- 날짜 더하기/빼기
SELECT NOW() + INTERVAL 1 DAY;    -- 내일
SELECT NOW() - INTERVAL 30 MINUTE; -- 30분 전
SELECT DATE_ADD(NOW(), INTERVAL 1 MONTH);  -- 다음 달

-- 특정 날짜 생성
SELECT DATE('2024-05-17');
SELECT STR_TO_DATE('17/05/2024', '%d/%m/%Y');
```

## PostgreSQL 동일 기능

```sql
-- PostgreSQL
SELECT NOW();            -- timestamp with time zone
SELECT CURRENT_DATE;     -- date only
SELECT CURRENT_TIME;     -- time only
SELECT NOW() AT TIME ZONE 'Asia/Seoul';  -- 타임존 변환
```

## 관련

- [[SQL]]
- [[MySQL(MariaDB)]]
- [[PostgreSQL]]
- [[CREATE]]
- [[INSERT]]
