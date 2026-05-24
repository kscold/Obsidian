- 청킹 = **원본 문서를 [[임베딩(Embedding)|임베딩]] · 검색 단위로 잘게 쪼개는 작업**. [[RAG(Retrieval-Augmented Generation)]] 품질의 50% 이상을 좌우한다.
- 청크가 너무 작으면 맥락이 부족해 LLM이 답을 못 만들고, 너무 크면 노이즈·비용·임베딩 한도(8K 토큰) 초과 문제가 생긴다.

## 대표 전략

### 1. Fixed-size (고정 크기)

```python
from langchain_text_splitters import CharacterTextSplitter
splitter = CharacterTextSplitter(chunk_size=500, chunk_overlap=50)
```

- 가장 단순. 문장·단락 경계를 무시해 의미가 잘리기 쉬움.

### 2. Recursive Character Splitter (가장 흔히 권장)

```python
from langchain_text_splitters import RecursiveCharacterTextSplitter
splitter = RecursiveCharacterTextSplitter(
    chunk_size=500,
    chunk_overlap=50,
    separators=["\n\n", "\n", ". ", " ", ""],  # 위에서부터 시도
)
```

- 단락 → 문장 → 단어 순으로 자른다. 의미를 가장 덜 깨뜨림.

### 3. Semantic Chunking

```python
from langchain_experimental.text_splitter import SemanticChunker
splitter = SemanticChunker(OpenAIEmbeddings(), breakpoint_threshold_type="percentile")
```

- 인접 문장의 임베딩 유사도가 급락하는 지점을 경계로 삼는다.
- 의미 단위가 매우 잘 보존되지만, 임베딩 호출 비용이 든다.

### 4. Document-structure aware

- Markdown은 `#` 헤더, HTML은 태그, 코드는 함수·클래스 단위로 자른다.
- `MarkdownHeaderTextSplitter`, `PythonCodeTextSplitter` 등 전용 splitter 사용.

### 5. Parent-Child / Small-to-Big

- 검색은 **작은 청크**로(정밀), LLM에 넘길 때는 **부모 청크 또는 주변 윈도우**를 함께 전달(맥락).

```python
from langchain.retrievers import ParentDocumentRetriever
# child_splitter: 200자, parent_splitter: 2000자
```

## 권장 파라미터 (한국어 기준)

| 도메인 | chunk_size (토큰) | overlap |
|--------|--------------------|---------|
| FAQ, 짧은 QA | 200~300 | 20~50 |
| 매뉴얼·정책 문서 | 400~600 | 50~100 |
| 코드·기술 문서 | 800~1500 | 100 |
| 법률·계약서 | 600~1000 + 구조 보존 | 100~150 |

- 한국어는 영어보다 토큰화 효율이 낮아(한 글자가 1~3 토큰) 글자 수 기준은 토큰 기준의 약 1/2.

## 메타데이터 동봉이 핵심

```python
chunk_metadata = {
    "source": "정책_2024.pdf",
    "page": 7,
    "section": "환불 규정",
    "doc_id": "policy-2024-07",
}
```

- 출처 페이지·섹션을 함께 저장해야 [[RAG(Retrieval-Augmented Generation)|답변에 출처 인용]]이 가능하다.

## 흔한 실수

- 표(table)나 코드 블록 한가운데서 잘림 → 구조 기반 splitter나 전용 PDF 파서(Unstructured, LlamaParse) 사용.
- 청크에 헤더 정보가 없어 "이 청크가 어느 문단의 일부인지" 모름 → 헤더 prepend 패턴 사용.
- overlap = 0 → 경계 정보가 한쪽에만 남아 검색 누락.
