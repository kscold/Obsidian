---
title: Registry Pattern
created: 2026-06-26
tags:
  - python
  - pattern
---

# Registry Pattern

Registry Pattern은 문자열 키나 타입을 실제 함수, 클래스, 설정으로 매핑하는 패턴이다.

## 예

```python
REGISTRY = {
    "BLOCK_042": normalize_block_042,
    "TVS0001": normalize_tvs0001,
}

normalizer = REGISTRY.get(block_type)
```

## 왜 쓰나

- if/elif 체인이 길어지는 것을 막는다.
- 새 블록 추가 시 등록 위치가 명확해진다.
- block type, provider, tool name 같은 문자열을 실행 객체로 바꾸기 쉽다.

## exact와 prefix

프로젝트에서는 block type 정확 매칭을 먼저 보고, 없으면 prefix 매칭을 쓴다.

```text
BLOCK_042 -> exact normalizer
TVS*    -> prefix normalizer
```

## 한 줄 정리

Registry Pattern은 **문자열 키를 함수나 설정 객체로 연결해 확장 지점을 명확히 만드는 패턴**이다.

## 관련

- [[Block Manifest]]
- [[Post-merge Normalizer]]
- [[Factory Pattern]]
