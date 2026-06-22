---
title: External Information MAS
created: 2026-06-22
tags:
  - ai-agent
  - multi-agent
  - rag
  - architecture
---

# External Information MAS

External Information MAS는 여러 에이전트가 내부 DB, 로컬 파일, 외부 API, 웹 검색 등을 나눠 조회한 뒤 결과를 종합하는 멀티 에이전트 구조다.

## 왜 필요한가

LLM은 학습된 지식만으로 답하면 최신성, 정확성, 조직 내부 지식에서 약하다.

그래서 에이전트에게 정보 출처별 역할을 나눠준다.

## 구조

```mermaid
flowchart TD
    User["사용자 질문"]
    Router["Router / Supervisor"]
    DB["DB Agent<br/>내부 데이터"]
    File["File Agent<br/>CSV/PDF/문서"]
    Web["Web Agent<br/>외부 API/검색"]
    Report["Report Agent<br/>종합"]

    User --> Router
    Router --> DB
    Router --> File
    Router --> Web
    DB --> Report
    File --> Report
    Web --> Report
```

## 실습과 연결

뉴스/주가 리포트 실습은 External Information MAS의 작은 형태다.

| 에이전트 | 정보 출처 |
|---|---|
| `news_agent` | Google News RSS |
| `stock_agent` | yfinance API |
| `reporter` | 수집 결과 종합 |

## 내부 정보 우선 전략

운영 환경에서는 보통 내부 정보를 먼저 본다.

```mermaid
flowchart TD
    Q["질문"]
    Internal["내부 DB/문서 검색"]
    Check["충분한가?"]
    External["외부 API/웹 검색"]
    Answer["답변 생성"]

    Q --> Internal --> Check
    Check -->|충분| Answer
    Check -->|부족| External --> Answer
```

## 설계 포인트

- 내부 데이터와 외부 데이터를 구분해서 출처를 남긴다.
- 최신성이 필요한 정보는 API나 웹 검색으로 보강한다.
- 개인/회사 내부 데이터는 외부 검색 프롬프트에 섞이지 않게 한다.
- 결과를 종합하는 reporter 노드에서 출처와 불확실성을 함께 정리한다.

## 관련

- [[RAG(Retrieval-Augmented Generation)]]
- [[Parallel Agent Fan-out]]
- [[Serial Agent Pipeline]]
- [[Fallback]]
- [[Observability]]
