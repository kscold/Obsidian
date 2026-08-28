---
title: MongoDB 문서 모델과 BSON
created: 2026-08-28
tags:
  - database
  - mongodb
---

# MongoDB 문서 모델과 BSON

- [[MongoDB]]가 저장하는 것은 JSON이 아니라 **BSON(Binary JSON)** 이다. JSON처럼 생겼지만 **타입이 더 많고 이진으로 저장**된다.

## BSON이 JSON보다 갖는 것

| 타입 | 뜻 | 파이썬 |
|---|---|---|
| `ObjectId` | 12바이트 고유 식별자 | `bson.ObjectId` |
| `Date` | 밀리초 정밀도 UTC 시각 | `datetime` |
| `Int32` / `Int64` / `Double` | 정수·실수 구분 | `int` / `float` |
| `Decimal128` | 정확한 소수 (금액) | `Decimal` |
| `Binary` | 바이너리 blob | `bytes` |
| `Null` / `Boolean` / `Array` / `Object` | JSON과 동일 | — |

JSON에는 날짜 타입이 없다. **BSON에는 있다** — 그래서 `{"created_at": {"$gte": since}}` 같은 범위 질의가 문자열 비교가 아니라 진짜 시각 비교로 동작한다.

## `_id` — 모든 문서의 기본 키

```javascript
{ "_id": ObjectId("66cf1e2a5b3c9a0012f4a1b2"), "block_id": "TSM0004", ... }
```

- **모든 문서에 반드시 있다.** 안 주면 드라이버가 `ObjectId`를 만들어 넣는다.
- 유니크 인덱스가 자동으로 걸려 있다. 지우거나 바꿀 수 없다.
- 직접 지정해도 된다. 의미 있는 키가 이미 있으면 그걸 `_id`로 쓰는 게 인덱스 하나를 아끼는 방법이다.

### ObjectId 구조

```text
66cf1e2a  5b3c9a0012  f4a1b2
[4B 시각] [5B 랜덤]   [3B 카운터]
```

- **시각이 앞에 있어서 대략 생성순으로 정렬된다.** `_id` 정렬 ≈ 생성 시각 정렬.
- 분산 환경에서도 충돌하지 않는다 (중앙 채번 불필요).
- 단, **정확한 생성 시각의 근거로 쓰지 않는다.** 시계가 다른 서버끼리는 어긋난다. 시각이 중요하면 `created_at` 필드를 따로 둔다.

## 우리 코드가 `_id`를 다루는 방식

```python
doc = await db.blocks.find_one({"block_id": "TSM0004"}, {"_id": 0})
```

**`_id`를 거의 항상 [[projection|제외]]한다.** 이유가 있다.

- `ObjectId`는 **JSON 직렬화가 안 된다.** gRPC나 API로 그대로 내보내면 터진다.
- 우리에겐 이미 의미 있는 키(`block_id`, `session_id`)가 있다. `_id`는 내부 사정이다.

내보내야 할 때는 문자열로 바꾼다.

```python
from bson import ObjectId

def _serialize(value):
    if isinstance(value, ObjectId):
        return str(value)
    return value
```

## 중첩과 배열

문서 안에 문서를, 배열을 그대로 넣을 수 있다. 이게 [[JOIN]] 없이 사는 방법이다.

```javascript
{
  "session_id": "s-1",
  "messages": [
    { "role": "user", "content": "회귀 분석 해줘", "ts": ISODate("...") },
    { "role": "assistant", "content": "...", "ts": ISODate("...") }
  ],
  "summary": { "text": "...", "turns": 12 }
}
```

- 조회는 **dot notation**: `{"summary.turns": {"$gte": 10}}` → [[MongoDB 배열 쿼리]]
- 한 번의 읽기로 전부 가져온다. 대화 세션처럼 **항상 통째로 쓰는 데이터**에 최적이다.

## 16MB 제한

**문서 하나는 16MB를 넘을 수 없다.** 이게 [[MongoDB 데이터 모델링]]의 가장 큰 제약이다.

- 대화 메시지를 무한히 `$push`하면 언젠가 넘는다. 요약으로 접거나 별도 컬렉션으로 뺀다.
- 배열이 계속 자라는 설계는 **크기뿐 아니라 성능도** 문제다. 문서를 갱신할 때마다 전체를 다시 쓴다.

## 한 줄 정리

BSON은 **타입이 있는 이진 JSON**이고, `_id`/ObjectId는 내부 키라 외부로 내보낼 땐 빼거나 문자열로 바꾼다. 문서 하나는 16MB까지다.

## 관련

- [[MongoDB]]
- [[MongoDB 배열 쿼리]]
- [[MongoDB 데이터 모델링]]
- [[projection]]
- [[MongoDB CRUD 메서드]]
- [[json.dumps]]
- [[MongoDB MOC]]
