---
title: Dataset Profiling
created: 2026-06-26
tags:
  - ai-agent
  - data
  - profiling
---

# Dataset Profiling

Dataset Profiling은 원천 metadata를 구조화된 데이터셋 신호로 바꾸는 과정이다. AI Agent가 데이터 분석 workflow를 만들 때, LLM이 컬럼명만 보고 판단하지 않도록 돕는다.

## 산출하는 신호

- 행 수 구간
- 수치형/범주형/날짜형 컬럼 목록
- 분류 타깃 후보
- 회귀 타깃 후보
- 고카디널리티 컬럼
- 시계열 후보 컬럼
- ID-like 컬럼
- 결측 비율

## 어디에 쓰나

- [[Data-aware Prompting]]
- [[Profile-based Input Sanitization]]
- target/feature 추천
- recipe selection
- warning generation

## 핵심 설계

LLM에게 보여주는 데이터 설명과 시스템이 실제로 쓰는 데이터 신호는 같은 기준에서 나와야 한다. 그래야 prompt와 검증 로직이 서로 다른 결론을 내리지 않는다.

관련: [[Data-aware Prompting]], [[Profile-based Input Sanitization]]
