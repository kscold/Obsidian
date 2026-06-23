- 임베딩 = **텍스트·이미지 같은 비정형 데이터를 의미를 보존한 채 고정 차원의 실수 벡터로 변환**한 것.
- 의미가 비슷한 두 문장은 벡터 공간에서 가까이 놓인다 — 이걸로 [[벡터 데이터베이스|벡터 검색]], [[RAG(Retrieval-Augmented Generation)]], 분류, 클러스터링이 가능해진다.
- 출력 차원은 모델별로 다름: OpenAI `text-embedding-3-small`은 1536, `large`는 3072, BGE-M3는 1024.

## 핵심 직관

```
"강아지가 짖는다"   → [0.12, -0.88, ..., 0.04]
"개가 멍멍한다"     → [0.13, -0.85, ..., 0.05]   ← 코사인 유사도 ≈ 0.98
"파이썬 문법 정리" → [0.74, 0.20, ..., -0.31]   ← 유사도 ≈ 0.05
```

## 유사도 계산

```python
import numpy as np

def cosine_similarity(a: np.ndarray, b: np.ndarray) -> float:
    return float(np.dot(a, b) / (np.linalg.norm(a) * np.linalg.norm(b)))

# 임베딩이 L2 정규화돼 있으면 dot product = cosine
```

- **Cosine similarity** — 각도 기반, 가장 흔함.
- **Dot product** — 임베딩이 정규화돼 있으면 cosine과 같음.
- **Euclidean distance** — 거리가 작을수록 유사. 잘 안 씀.
- 자세한 직관은 [[코사인 유사도]] 참고.

## OpenAI 임베딩 호출

```python
from openai import OpenAI
client = OpenAI()

res = client.embeddings.create(
    model="text-embedding-3-small",
    input=["강아지가 짖는다", "개가 멍멍한다"],
)
vectors = [d.embedding for d in res.data]
```

## 한국어 친화 임베딩 모델

| 모델 | 차원 | 특징 |
|------|------|------|
| `text-embedding-3-large` (OpenAI) | 3072 | 범용, 한국어도 우수 |
| `BAAI/bge-m3` | 1024 | 다국어, dense+sparse+colbert 동시 출력 |
| `jhgan/ko-sroberta-multitask` | 768 | 한국어 특화, 로컬 호스팅 가능 |
| `intfloat/multilingual-e5-large` | 1024 | 다국어, 비용 효율 |

## 자주 빠지는 함정

- **모델을 바꾸면 전체 재인덱싱 필수** — 다른 모델의 벡터는 같은 공간에 있지 않다.
- **쿼리와 문서에 같은 모델을 써야** 함. 모델 따라 prefix가 다른 경우도 있음 (`"query: ..."`, `"passage: ..."`).
- 입력 길이 제한이 있다 (OpenAI 8191 토큰). [[청킹(Chunking)]] 필요.

## 임베딩이 부족할 때

- 정확한 키워드·고유명사 매칭이 약하다 → [[Hybrid Search]]로 BM25 보강.
- 의미는 비슷한데 답이 아닌 문서가 끼어든다 → [[Reranking]] 추가.

## 평가 지표와의 연결

- [[BERTScore]]는 생성문과 참조문을 [[BERT]] 임베딩으로 바꾼 뒤 [[코사인 유사도]]로 의미가 얼마나 비슷한지 본다.
- 그래서 BLEU/ROUGE처럼 단어가 그대로 겹치는지만 보는 지표보다 표현 변화에 강하다.
