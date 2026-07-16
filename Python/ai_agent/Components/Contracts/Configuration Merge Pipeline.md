---
title: Configuration Merge Pipeline
created: 2026-06-26
tags:
  - ai-agent
  - configuration
  - guardrails
---

# Configuration Merge Pipeline

Configuration Merge Pipeline은 기존 설정, 사용자 명시 변경, LLM 추천, 시스템 기본값, 환경 메타데이터를 하나의 최종 설정으로 합치는 과정이다.

## 왜 필요한가

AI가 만든 새 설정만 보면 안 된다. 사용자가 유지하려는 값, 시스템이 요구하는 기본값, 실행 환경에서 필요한 보정값이 서로 다른 우선순위를 갖기 때문이다.

## 일반적인 우선순위

1. 사용자가 명시적으로 고정한 값
2. 현재 세션에서 방금 요청한 변경
3. 기존 설정
4. LLM 또는 specialist 추천값
5. contract default
6. 실행 직전 normalizer가 채운 안전값

## 파이프라인

1. 기존 설정을 dict로 정규화한다.
2. 명시 변경을 overlay한다.
3. 추천값과 기본값을 병합한다.
4. [[Post-merge Normalizer]]로 실행 친화 형태로 보정한다.
5. [[No-op Change Guard]]로 의미 없는 재적용을 차단한다.

## 한 줄 정리

Configuration Merge Pipeline은 **사람의 의도, 기존 상태, AI 추천, 시스템 계약을 충돌 없이 하나의 실행 설정으로 만드는 과정**이다.

관련: [[Pre-validation Normalizer]], [[Post-merge Normalizer]], [[User Preference Locking]]
