---
title: JOIN
created: 2026-08-28
tags:
  - database
  - sql
---

# JOIN

- JOIN은 **두 표를 공통 키로 이어 붙이는** [[SQL]] 연산이다.
- 데이터를 중복 없이 여러 표로 쪼개 두고(정규화), 필요할 때 합치는 게 관계형의 전제다.

```sql
SELECT u.name, w.title
FROM users u
JOIN workflows w ON w.user_id = u.id;
```

## 종류

| 종류 | 남는 행 |
|---|---|
| `INNER JOIN` | 양쪽에 **다 있는** 것만 |
| `LEFT JOIN` | 왼쪽은 다 남고, 오른쪽은 없으면 `NULL` |
| `RIGHT JOIN` | 그 반대 |
| `FULL OUTER JOIN` | 양쪽 다 남김 (MySQL/MariaDB는 미지원, UNION으로 흉내) |

## 실무 감각

- JOIN 키에는 [[인덱스(Index)|인덱스]]가 있어야 한다. 없으면 오른쪽 표를 행마다 풀 스캔한다.
- JOIN이 3~4개를 넘어가면 대개 모델링을 다시 볼 신호다.
- [[MongoDB]]에는 이 개념이 사실상 없다. 그래서 **필요한 걸 문서 안에 미리 넣어 두는(비정규화)** 방식으로 푼다.

## 한 줄 정리

JOIN은 **쪼개 둔 표를 질의 시점에 키로 합치는 것**이고, 문서 DB는 대신 미리 합쳐 저장한다.

## 관련

- [[SQL]]
- [[RDBMS vs Document DB]]
- [[인덱스(Index)]]
