---
title: Block Contract
created: 2026-06-26
tags:
  - ai-agent
  - contract
  - workflow
---

# Block Contract

Block Contract는 워크플로우 블록이 어떤 입력을 받고, 어떤 옵션을 요구하며, 어떤 출력 슬롯을 만드는지 정의한 계약이다.

LLM이 블록을 추천하더라도 실제 실행 가능성은 계약으로 검증해야 한다.

## 포함 내용

| 영역 | 예 |
|---|---|
| input_contract | 허용 upstream, 금지 upstream, 입력 슬롯 수 |
| option_contract | 필수 옵션, 타입, 기본값, 허용값 |
| output_contract | 출력 슬롯, terminal 여부 |
| preconditions | 실행 전 조건 |
| failure_patterns | 흔한 실패와 수정 가이드 |

## 왜 중요한가

- LLM이 잘못된 컬럼 타입을 고를 수 있다.
- 시각화 블록과 모델링 블록은 필요한 옵션이 다르다.
- dual-slot 블록은 입력 연결 수가 맞아야 한다.
- terminal 블록은 뒤에 연결하면 안 되는 경우가 있다.

## 프롬프트에도 사용

계약은 검증뿐 아니라 LLM 프롬프트에도 들어간다.

- 실행 조건
- 옵션 스키마
- 실패 패턴
- R 함수/API 실행 흐름

## 한 줄 정리

Block Contract는 **AI가 추천한 블록이 실제 워크플로우에서 실행 가능한지 판단하는 블록별 인터페이스 계약**이다.

## 관련

- [[Contract-first Workflow]]
- [[Contract Guardrail Pipeline]]
- [[Block Manifest]]
- [[Workflow Action]]
- [[Pre-validation Normalizer]]
