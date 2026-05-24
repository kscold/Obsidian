- Reranking = **1차 검색(벡터/Hybrid)로 후보를 많이 뽑은 뒤, 더 정밀한 모델로 재정렬해 top-k를 가려내는 단계**.
- 1차 검색은 query·doc을 **각각** 임베딩한 뒤 코사인 유사도를 쓰기 때문에 빠르지만 부정확. Reranker는 query+doc을 **함께** 모델에 넣어 점수를 매기므로 느리지만 훨씬 정확하다.

## Bi-encoder vs Cross-encoder

| 구분 | Bi-encoder (1차 검색) | Cross-encoder (Reranker) |
|------|------------------------|--------------------------|
| 입력 | query, doc 따로 인코딩 | (query, doc) 함께 |
| 출력 | 두 벡터의 유사도 | 직접 점수 (0~1) |
| 속도 | 매우 빠름 (벡터 캐싱 가능) | 느림 (매번 forward) |
| 정확도 | 보통 | 높음 |
| 용도 | 수백만 후보에서 top-50 | top-50 → top-5 |

## 구현 예 (Cohere Rerank)

```python
import cohere
co = cohere.Client(api_key="...")

# 1차 검색 결과 50개를 받았다고 가정
candidates = ["문서1...", "문서2...", ..., "문서50..."]

reranked = co.rerank(
    model="rerank-multilingual-v3.0",
    query="환불 정책이 뭐야?",
    documents=candidates,
    top_n=5,
)
for r in reranked.results:
    print(r.index, r.relevance_score)
```

## 오픈소스 Cross-encoder

```python
from sentence_transformers import CrossEncoder
model = CrossEncoder("BAAI/bge-reranker-v2-m3")
pairs = [(query, doc) for doc in candidates]
scores = model.predict(pairs)
top = sorted(zip(candidates, scores), key=lambda x: x[1], reverse=True)[:5]
```

- 추천 모델: `BAAI/bge-reranker-v2-m3` (다국어, 한국어 좋음), `jinaai/jina-reranker-v2`.

## LLM as a Reranker

- 비용을 감수할 수 있으면 [[LLM(Large Language Model)]]에 직접 "이 문서가 질문과 얼마나 관련 있나(0~10)?"를 묻는 방법.
- 가장 정확하지만 가장 비싸고 느림. 보통 cross-encoder로 충분.

## 효과

- 한 단계 추가로 RAG 답변 품질이 흔히 **10~30% 향상**. 가성비 가장 좋은 개선 포인트.
- Recall은 1차 검색이, Precision은 Reranker가 책임진다는 분업 구조.

## 주의

- Reranker는 비용·지연이 늘기 때문에 top-50→top-5처럼 **소수 후보**에만 적용.
- 한국어 도메인은 다국어 reranker가 영어 전용보다 거의 항상 낫다.
