---
title: projection
created: 2026-08-28
tags:
  - database
  - mongodb
---

# projection

- projection은 [[MongoDB]] 조회에서 **어떤 필드만 받을지** 고르는 두 번째 인자다. [[SQL]]의 `SELECT 열 목록`에 해당한다.

```javascript
db.blocks.find({}, { _id: 0 })                   // _id 빼고 전부
db.blocks.find({}, { block_id: 1, name: 1 })     // 이 둘만 (+ _id 기본 포함)
```

- `1` = 포함, `0` = 제외. **둘을 섞을 수 없다.** 예외가 `_id`뿐이라 `{ _id: 0, name: 1 }`은 된다.

## 왜 신경 쓰나

- 문서가 크면 네트워크와 메모리를 그대로 먹는다. 벡터나 원본 payload가 든 문서라면 차이가 크다.
- **커버링 인덱스**: 필요한 필드가 전부 [[인덱스(Index)|인덱스]] 안에 있으면 DB가 문서를 아예 안 읽는다.
- 도메인 관점에서도 의미가 있다. 필요한 필드만 뽑는 순간 그게 **projection = 원본의 축소 사본**이 된다. 프로젝트에서 "Catalog projection"이라 부르는 것도 같은 뜻이다.

## 벡터 쪽 대응

[[Qdrant]]에도 같은 개념이 있다. `with_payload`, `with_vectors`가 그 자리다. 특히 `with_vectors=False`는 1024차원 배열을 안 받겠다는 뜻이라 효과가 크다.

## 한 줄 정리

projection은 **필요한 필드만 받는 선언**이고, 성능과 도메인 모델링 양쪽에 걸쳐 있다.

## 관련

- [[MongoDB]]
- [[MongoDB 쿼리 연산자]]
- [[Qdrant Point와 Payload]]
