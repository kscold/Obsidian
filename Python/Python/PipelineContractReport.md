---
title: PipelineContractReport
created: 2026-06-26
tags:
  - python
  - ai-agent
  - contract
---

# PipelineContractReport

PipelineContractReport는 파이프라인 전체에서 발견된 계약 위반을 모아두는 값 객체이다.

이 노트는 Python 문법보다는 `field(default_factory)` 예시를 연결하기 위한 작은 구현 노트이다.

## 핵심 구조

```python
@dataclass
class PipelineContractReport:
    violations: list[ContractViolation] = field(default_factory=list)
```

## 왜 default_factory인가

각 report 인스턴스는 자기만의 violation list를 가져야 한다.

`[]`를 기본값으로 직접 두면 인스턴스끼리 같은 list를 공유할 위험이 있다.

## 관련

- [[field(default_factory)]]
- [[Block Contract]]
- [[Contract Guardrail Pipeline]]
