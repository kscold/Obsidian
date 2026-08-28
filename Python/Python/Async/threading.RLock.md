---
title: threading.RLock
created: 2026-06-26
tags:
  - python
  - concurrency
---

# threading.RLock

`threading.RLock`은 같은 thread가 여러 번 획득할 수 있는 reentrant lock이다. 일반 `Lock`은 같은 thread가 이미 잡은 lock을 다시 잡으려 하면 deadlock이 날 수 있지만, `RLock`은 획득 횟수를 세고 같은 횟수만큼 release되면 풀린다.

## 실무에서의 쓰임

`CanvasSessionStore`는 저장, 조회, atomic update를 모두 lock으로 감싼다. 특히 `update_locked_options_atomic`처럼 같은 저장소 상태를 읽고 수정하고 다시 쓰는 구간은 한 lock 안에 있어야 lost update를 막을 수 있다.

관련: [[Session Context Store]], [[User Preference Locking]]
