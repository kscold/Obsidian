---
title: Components 개념 MOC
created: 2026-07-15
tags:
  - ai-agent
  - moc
  - architecture
---

# Components 개념 MOC

AI Agent를 구성하는 재사용 가능한 컴포넌트를 역할별로 묶은 인덱스이다. 특정 프로젝트의 기능명보다, 다른 Agent에도 옮겨 쓸 수 있는 설계 원칙을 먼저 본다.

## Agent Design

- [[Intent Classification]]
- [[Planning]]
- [[Capability Catalog]]
- [[Prompt-to-Action Planning]]
- [[Specialist Agent Pattern]]
- [[Reflection]]

## Context

- [[Context Engineering]]
- [[Prompt Engineering]]
- [[System Prompt]]
- [[Data-aware Prompting]]
- [[Prompt Caching]]

## Contracts and Safety

- [[Structured Output]]
- [[Block Contract]]
- [[Guardrails]]
- [[Contract Guardrail Pipeline]]
- [[Tool Execution Policy]]
- [[Human-in-the-loop]]

## Runtime

- [[LLM Provider 추상화]]
- [[LLM Routing]]
- [[Async Resource Lifecycle]]
- [[Streaming]]
- [[SSE 기반 Agent Streaming]]

## Memory

- [[Memory]]
- [[Session Context Store]]
- [[User Preference Locking]]
- [[Negative Feedback Memory]]

## Interoperability

- [[MCP(Model Context Protocol)]]
- [[MCP Security]]
- [[A2A 프로토콜]]
- [[SKILL]]

## Observability and Quality

- [[Observability]]
- [[Tool Observability Wrapping]]
- [[Trajectory]]
- [[Evaluation]]
- [[Grounded Generation]]
- [[LLM-as-Judge]]

## Data and Multimodal

- [[Dataset Profiling]]
- [[OCR]]
- [[VLM]]
- [[SQLite 데이터 소스]]
