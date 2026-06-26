---
title: copy.deepcopy
created: 2026-06-26
tags:
  - python
  - method
---

# copy.deepcopy

`copy.deepcopy(obj)`는 중첩 객체까지 새로 복사한다. list 안의 dict, dict 안의 list처럼 내부 구조가 있는 데이터를 안전하게 수정해야 할 때 쓴다.

## 얕은 복사와 차이

`dict(obj)`나 `list(obj)`는 바깥 컨테이너만 새로 만든다. 내부 dict/list는 같은 객체를 가리킬 수 있다. `deepcopy`는 내부 객체까지 재귀적으로 복사한다.

## 실무에서의 쓰임

[[Specialist Agent Pattern]]이나 workflow mutation 로직에서는 이전 상태를 깊은 복사한 뒤 draft를 수정하는 경우가 많다. 그래야 중간 보정 실패나 계약 위반이 원본 상태를 오염시키지 않는다.

관련: [[Lightweight Intent Handler]]
