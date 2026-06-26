---
title: Data-aware Prompting
created: 2026-06-26
tags:
  - ai-agent
  - prompt
  - data
---

# Data-aware Prompting

Data-aware Prompting은 LLM에게 사용자 요청만 주는 것이 아니라, 데이터셋의 구조와 통계적 신호를 함께 제공하는 프롬프트 설계 기법이다.

## 포함할 수 있는 정보

- 행/컬럼 수와 타입 분포
- 사용자 발화에 언급된 컬럼
- 결측 summary
- 타깃 후보
- 고카디널리티/ID-like/시계열 후보
- 컬럼별 간단한 통계

## 장점

- LLM이 컬럼명을 더 정확히 해석한다.
- 분석 목적에 맞는 target과 feature를 고르기 쉬워진다.
- 실행 불가능한 추천이 줄어든다.
- [[Profile-based Input Sanitization]]과 함께 쓰면 더 안전하다.

## 토큰 예산

컬럼이 많은 데이터셋에서는 전체 metadata를 모두 넣지 않는다. 중요한 컬럼만 우선순위로 넣고, 잘린 컬럼 수를 명시하는 방식이 좋다.

관련: [[Prompt Context Builder]], [[Dataset Profiling]], [[Prompt Engineering]]
