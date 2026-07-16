---
title: MCP Security
created: 2026-07-15
tags:
  - ai-agent
  - mcp
  - security
  - tool-calling
---

# MCP Security

MCP Security는 [[MCP(Model Context Protocol)]] 서버를 붙일 때 서버·도구·도구 결과를 신뢰 경계로 다루는 설계다. MCP 서버가 연결되었다는 사실은, 그 서버가 모델에게 보여 줄 설명과 실제 실행 권한을 모두 신뢰해도 된다는 뜻이 아니다.

## 위협 모델

| 경계 | 대표 위험 | 핵심 대응 |
|---|---|---|
| 서버 등록 | 악성 로컬 명령 실행, 과도한 파일·네트워크 권한 | 명시적 동의, 정확한 실행 명령 검토, sandbox와 최소 권한 |
| 도구 설명 | tool description 안의 prompt injection | 설명을 정책이 아닌 비신뢰 입력으로 취급 |
| 원격 인증 | confused deputy, 토큰 오용, 과도한 scope | OAuth 검증, client별 동의, audience와 scope 확인 |
| URL 탐색 | SSRF, redirect를 통한 내부망 접근 | HTTPS, private IP 차단, redirect 재검증, egress 정책 |
| 도구 결과 | 비밀 유출, 지시 삽입, 잘못된 데이터 | 결과 schema 검증, 민감값 마스킹, 모델 재투입 전 정제 |

## 실행 전 신뢰 사슬

~~~mermaid
flowchart LR
    Register["서버 등록 / 발견"] --> Trust["서버 신뢰 및 권한 검토"]
    Trust --> Auth["인증과 최소 scope"]
    Auth --> Policy["도구별 실행 정책"]
    Policy --> Invoke["MCP tool 호출"]
    Invoke --> Validate["결과 검증·마스킹"]
    Validate --> Trace["감사 로그와 사용자 표시"]
~~~

각 단계는 이전 단계를 대체하지 않는다. OAuth 인증이 되었다고 위험한 파일 삭제 도구를 자동 실행해도 되는 것은 아니며, 사용자 승인이 있었다고 응답 안의 비밀을 LLM 컨텍스트에 그대로 넣어도 되는 것도 아니다.

## 로컬 MCP 서버

- 연결 전에 실행할 **전체 명령과 인자**를 표시하고 사용자 동의를 받는다.
- stdio 서버도 클라이언트 권한으로 실행되므로 파일 시스템, 네트워크, 환경 변수를 최소화한다.
- 가능한 한 sandbox, 전용 작업 디렉터리, 읽기 전용 mount, egress 제한을 사용한다.
- 서버별 allowlist와 버전·해시 검증 정책을 둔다.
- 새 서버의 tool 목록이 바뀌면 재검토한다.

## 원격 MCP 서버

- 사용자별 데이터나 쓰기 도구에는 검증된 OAuth 흐름과 최소 scope를 사용한다.
- access token의 issuer, audience, 만료, scope를 서버가 직접 검증한다.
- token passthrough처럼 클라이언트 토큰을 검증 없이 하위 API로 넘기지 않는다.
- redirect URI와 OAuth discovery URL은 정확히 검증하고, production에서는 HTTPS를 강제한다.
- Mcp-Session-Id 같은 세션 식별자를 인증 수단으로 사용하지 않는다.

## LLM과 도구 사이의 정책

모델은 MCP tool의 설명을 보고 호출을 제안할 수 있지만, 실행 권한은 시스템 정책이 정한다.

1. 도구를 읽기·제한적 쓰기·외부 쓰기·고위험 쓰기로 분류한다.
2. 현재 사용자, 테넌트, 데이터 범위, 인자를 확인한다.
3. 위험한 쓰기는 [[Human-in-the-loop|승인]]과 idempotency key를 요구한다.
4. 도구 결과는 schema와 출처를 기록하고, 민감값을 정제한 뒤 모델에 전달한다.

이 계층은 [[Tool Execution Policy]]와 [[Guardrails]]의 책임이다. MCP는 도구를 연결하는 표준이고, 안전한 실행 정책을 자동으로 제공하지 않는다.

## 운영 체크리스트

- 서버, 도구, 권한, 소유자, 마지막 검토일을 inventory로 관리한다.
- tool call마다 요청자, 승인, 정책 결과, 입력 요약, 결과 요약, trace ID를 남긴다.
- 인증 헤더, access token, 비밀값, 원문 민감 데이터는 로그에서 제거한다.
- 새 tool description과 server update를 공급망 변경으로 취급한다.
- tool 결과에 포함된 지시문은 system prompt보다 낮은 신뢰도로 취급한다.

## 관련

- [[MCP(Model Context Protocol)]]
- [[A2A 프로토콜]]
- [[Tool Calling]]
- [[Tool Execution Policy]]
- [[Guardrails]]
- [[Observability]]

## 공식 문서

- [MCP Security Best Practices](https://modelcontextprotocol.io/docs/tutorials/security/security_best_practices)
- [MCP Authorization](https://modelcontextprotocol.io/docs/tutorials/security/authorization)
