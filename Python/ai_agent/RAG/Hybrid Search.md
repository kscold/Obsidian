- Hybrid Search = **벡터(의미) 검색 + 키워드(어휘) 검색**을 함께 수행하고 점수를 결합해 둘의 장점을 모두 취하는 방식.
- 의미 검색은 동의어·맥락에 강하지만 **고유명사, 코드, 정확한 용어, 숫자**에는 약하다. BM25 같은 키워드 검색이 그 빈틈을 메운다.

## 두 검색의 강·약

| 항목 | 벡터 (Dense) | 키워드 (Sparse, BM25) |
|------|--------------|------------------------|
| 동의어/패러프레이즈 | 강함 | 약함 |
| 고유명사·제품코드 | 약함 | 강함 |
| 다국어 | 모델 의존 | 약함 |
| 새 용어(OOD) | 강함 | 약함 |
| 짧은 키워드 질의 | 약함 | 강함 |

## 점수 결합 방식

### 1. Linear Combination

```python
final_score = α * vector_score + (1 - α) * bm25_score
# α는 보통 0.5~0.7
```

- 두 점수의 스케일이 달라 정규화 필요.

### 2. Reciprocal Rank Fusion (RRF) — 권장

```python
def rrf(ranks: dict[str, int], k: int = 60) -> float:
    return sum(1 / (k + r) for r in ranks.values())
```

- 각 검색의 **순위**만 사용 → 점수 스케일 문제 없음.
- 가장 안정적이고 튜닝 적게 든다.

## 구현 예 (Qdrant + BM25)

```python
from rank_bm25 import BM25Okapi

# BM25 인덱스
tokenized = [doc.split() for doc in corpus]
bm25 = BM25Okapi(tokenized)

# 벡터 인덱스는 Qdrant에 적재돼 있다고 가정
def hybrid_search(query: str, top_k: int = 10):
    vec_hits = qdrant.search(query_vec, limit=50)        # 의미
    bm25_hits = bm25.get_top_n(query.split(), corpus, n=50)  # 어휘

    ranks = {}
    for r, h in enumerate(vec_hits):  ranks.setdefault(h.id, {})["v"] = r
    for r, h in enumerate(bm25_hits): ranks.setdefault(h.id, {})["b"] = r

    fused = sorted(ranks.items(), key=lambda kv: rrf(kv[1]), reverse=True)
    return fused[:top_k]
```

## 한국어 BM25 주의

- 토크나이저가 핵심. 공백 분리는 너무 거칠다.
- `kiwi`, `mecab`, `okt` 같은 형태소 분석기 + 명사 위주 추출.

```python
from kiwipiepy import Kiwi
kiwi = Kiwi()
tokens = [t.form for t in kiwi.tokenize(text) if t.tag.startswith(("N", "V"))]
```

## 대안: Sparse Embedding (SPLADE, BGE-M3)

- BGE-M3는 한 모델에서 dense + sparse + colbert 벡터를 동시에 출력 → 별도 BM25 없이도 hybrid가 가능.

## 권장 조합

```
1차 검색: Hybrid (top-50)
   ↓
2차: [[Reranking|Cross-encoder Rerank]] (top-5)
   ↓
3차: LLM 생성
```

- 이 3단 구조가 production RAG의 거의 표준.
