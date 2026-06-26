---
title: Capability Catalog
created: 2026-06-26
tags:
  - ai-agent
  - tools
  - catalog
---

# Capability Catalog

Capability Catalog는 agent가 사용할 수 있는 tool, workflow block, skill, action type의 목록과 설명을 구조화해 둔 카탈로그이다.

## 역할

- LLM prompt에 사용 가능한 선택지를 제공한다.
- 사용자가 요청한 기능을 실제 capability에 매핑한다.
- frontend나 policy상 선택 불가능한 capability를 숨긴다.
- [[Prompt-to-Action Planning]]의 action 후보를 제한한다.

## 포함할 수 있는 정보

- ID
- 이름
- 카테고리
- 설명
- 입력/출력 스키마
- 선택 가능 여부
- 실행 비용
- 위험도
- 관련 예시

## RAG와의 관계

Capability가 많아지면 전체 목록을 매번 prompt에 넣기 어렵다. 이때 [[Hybrid RAG Search]]로 관련 capability만 검색해 prompt에 넣는다.

관련: [[Tool Calling]], [[Block Manifest]], [[Hybrid RAG Search]]
