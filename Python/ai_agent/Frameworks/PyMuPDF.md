---
title: PyMuPDF
created: '2026-06-23'
tags:
  - python
  - pdf
  - pymupdf
  - fitz
  - ocr
  - rag
---
# PyMuPDF

- PyMuPDF는 Python에서 PDF를 다루기 좋은 라이브러리다.
- 설치 패키지 이름은 `pymupdf`이고, 코드에서 import할 때는 보통 `fitz`라는 이름을 쓴다.
- 그래서 실습에서는 `pip install pymupdf` 후 `import fitz` 형태가 자주 나온다.

## 왜 fitz라고 import하나

```python
pip install pymupdf
```

```python
import fitz
```

- `fitz`는 PyMuPDF의 오래된 모듈 이름이다.
- 라이브러리 이름은 PyMuPDF지만, 사용할 때는 `fitz.open(...)`처럼 쓰는 경우가 많다.
- 따라서 `pymupdf라는 라이브러리를 설치하고 fitz로 import한다`고 기억하면 된다.

## 기본 텍스트 추출

```python
import fitz

pdf_path = "sample.pdf"
doc = fitz.open(pdf_path)

texts = []
for page_no, page in enumerate(doc, start=1):
    text = page.get_text("text")
    texts.append({
        "page": page_no,
        "text": text,
    })

full_text = "\n\n".join(item["text"] for item in texts)
print(full_text[:1000])
```

- `fitz.open(pdf_path)`는 PDF 파일을 연다.
- `page.get_text("text")`는 해당 페이지의 텍스트를 추출한다.
- 페이지 번호를 같이 저장하면 나중에 RAG 답변에서 출처를 표시하기 쉽다.

## 스캔 PDF 처리 흐름

```python
import fitz

pdf_path = "scanned.pdf"
doc = fitz.open(pdf_path)

for page_no, page in enumerate(doc, start=1):
    pix = page.get_pixmap(dpi=200)
    image_path = f"page_{page_no}.png"
    pix.save(image_path)
```

- 스캔 PDF는 `page.get_text()`로 텍스트가 거의 안 나올 수 있다.
- 이때는 PyMuPDF로 페이지를 이미지로 렌더링한 뒤 OCR 엔진에 넘긴다.
- OCR 엔진 예시는 Tesseract, EasyOCR, cloud OCR 등이 있다.

## RAG에서 쓰는 이유

- PDF를 페이지 단위로 읽을 수 있다.
- 텍스트 추출이 빠르고 코드가 단순하다.
- 페이지별 metadata를 붙이기 좋다.
- 이미지 PDF일 때도 페이지를 이미지로 변환해 OCR 파이프라인으로 넘길 수 있다.

## 한계

- 텍스트 레이어가 없는 스캔 PDF는 PyMuPDF만으로 글자를 정확히 읽을 수 없다.
- 표, 복잡한 레이아웃, 2단 문서에서는 텍스트 순서가 깨질 수 있다.
- OCR 품질은 이미지 해상도, 글자 선명도, 문서 언어에 영향을 많이 받는다.

## 한 줄 정리

- PyMuPDF는 PDF를 LLM/RAG에 넣기 전 텍스트를 뽑거나 페이지 이미지를 만드는 데 좋은 Python 라이브러리다.
- 설치는 `pymupdf`, 사용은 `import fitz`로 기억하면 된다.
