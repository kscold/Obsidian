---
title: Pydantic v2 메서드 사전
created: 2026-08-28
tags:
  - python
  - pydantic
  - reference
---

# Pydantic v2 메서드 사전

[[Pydantic]] v2에서 실제로 쓰는 메서드만. **v1과 이름이 전부 바뀌었다**는 게 첫 함정이다.

| v1 | v2 |
|---|---|
| `parse_obj()` | `model_validate()` |
| `parse_raw()` | `model_validate_json()` |
| `.dict()` | `model_dump()` |
| `.json()` | `model_dump_json()` |
| `.schema()` | `model_json_schema()` |
| `class Config:` | `model_config = ConfigDict(...)` |

## 검증 — 밖에서 안으로

```python
obj = Schema.model_validate(raw_dict)        # dict → 인스턴스
obj = Schema.model_validate_json(json_text)  # JSON 문자열 → 인스턴스 (파싱까지)
```

둘을 구분해서 쓰는 게 중요하다. [[Structured Output 정규화]]에서 dict 경로와 문자열 경로가 갈리는 이유다.

```python
try:
    return schema.model_validate(raw)
except ValidationError:
    content = raw.get("content")
    if isinstance(content, str) and content.strip():
        return schema.model_validate_json(content)
    raise
```

## 직렬화 — 안에서 밖으로

```python
obj.model_dump()                        # dict
obj.model_dump(exclude_none=True)       # None 필드 제외 — LLM 프롬프트에 넣을 때 유용
obj.model_dump(mode="json")             # datetime 등을 JSON 가능 타입으로
obj.model_dump_json()                   # JSON 문자열 ([[json.dumps]] 불필요)
Schema.model_json_schema()              # [[JSON Schema]] dict
```

## Field — 제약과 설명

```python
from pydantic import BaseModel, Field

class Plan(BaseModel):
    intent: Literal["analysis", "chat"] = Field(description="사용자 요청의 종류")
    confidence: float = Field(ge=0.0, le=1.0, description="0~1 확신도")
    blocks: list[str] = Field(default_factory=list, description="선택된 블록 ID")
```

- `description`은 [[JSON Schema]]로 그대로 실려 **모델이 읽는다.** 안 채우면 이름만 보고 추측한다.
- 가변 기본값은 반드시 `default_factory` ([[field(default_factory)]]). `= []`는 모든 인스턴스가 같은 리스트를 공유한다.
- `ge`/`le`/`min_length`/`pattern`이 그대로 스키마 제약이 된다.

## ConfigDict — 모델 전체 정책

```python
from pydantic import BaseModel, ConfigDict

class PromptSpec(BaseModel):
    model_config = ConfigDict(extra="forbid", frozen=True)
```

| 옵션 | 효과 |
|---|---|
| `extra="forbid"` | **선언 안 된 필드가 오면 에러.** 오타와 계약 이탈을 잡는다 |
| `extra="ignore"` (기본) | 조용히 버린다 — 계약 문서에는 위험 |
| `frozen=True` | 불변. 생성 후 대입 불가 ([[dataclass]]의 frozen과 같은 의도) |
| `populate_by_name=True` | alias와 필드명 둘 다 허용 |

**`extra="forbid"` + `frozen=True`** 조합이 계약 모델의 기본형이다. 값이 새로 들어오지도, 나중에 바뀌지도 않는다.

## Validator

```python
from pydantic import model_validator

class Range(BaseModel):
    start: int
    end: int

    @model_validator(mode="before")     # 검증 전 — raw 입력 손질
    @classmethod
    def normalize(cls, data):
        return data

    @model_validator(mode="after")      # 검증 후 — 필드 간 관계 확인
    def check(self):
        if self.end <= self.start:
            raise ValueError("end must be > start")
        return self
```

| mode | 시점 | 쓰임 |
|---|---|---|
| `before` | 타입 변환 **전** | 입력 모양 정규화 (LLM 출력 손질) |
| `after` | 전부 검증된 **뒤** | 필드 간 일관성 |

`field_validator`는 필드 하나, `model_validator`는 객체 전체다.

## TypeVar와 함께

```python
ModelT = TypeVar("ModelT", bound=BaseModel)

def coerce_structured_output(raw: object, schema: type[ModelT]) -> ModelT:
    ...
```

`bound=BaseModel`이면 **어떤 Pydantic 모델이든 받되, 넣은 타입 그대로 돌려준다**는 뜻이다 ([[TypeVar와 제네릭]]).

## 한 줄 정리

`model_validate`로 받고, `model_dump`로 내보내고, `ConfigDict(extra="forbid", frozen=True)`로 계약을 잠근다.

## 관련

- [[Pydantic]]
- [[JSON Schema]]
- [[Structured Output]]
- [[Structured Output 정규화]]
- [[TypeVar와 제네릭]]
- [[field(default_factory)]]
- [[Literal]]
- [[dataclass]]
- [[json.loads]]
