---
title: Block Manifest
created: 2026-06-26
tags:
  - ai-agent
  - registry
  - workflow
  - contract
---

# Block Manifest

Block Manifest는 한 블록의 등록 위치와 운영 메타를 한 곳에 모아 선언하는 구조이다.

프로젝트에서는 `BlockManifest` [[dataclass]]가 catalog, contract, normalizer, validator, specialist, RAG exemplar 같은 등록 지점을 묶는다.

## 왜 필요한가

새 블록을 추가할 때 등록 위치가 여러 곳으로 흩어진다.

- 카탈로그
- 계약
- 정규화기
- validator
- specialist prompt
- intent 예시
- relation metadata

Manifest는 이 흩어진 등록 지점을 하나의 선언으로 추적하게 해준다.

## audit

Manifest는 직접 실행 로직을 import하지 않고 dotted path 문자열을 가진다.

이러면 감사 스크립트가 다음을 확인할 수 있다.

- manifest가 가리키는 모듈이 실제 존재하는가
- catalog와 contract에 같은 block_id가 있는가
- normalizer/validator가 등록되어 있는가
- RAG 예시가 빠지지 않았는가

## 한 줄 정리

Block Manifest는 **한 블록이 시스템 어디에 어떻게 등록되어야 하는지 선언하는 단일 체크리스트**이다.

## 관련

- [[Registry Pattern]]
- [[Block Contract]]
- [[dataclass]]
- [[Contract-first Workflow]]
