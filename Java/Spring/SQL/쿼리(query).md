- `query`란 단어의 뜻은 문의하다, 질문하다라는 뜻으로 프로그래밍에서 이러한 문의는 요청, 즉 데이터베이스에 정보를 요청하는 일'을 말한다.

- 그렇게 때문에 정보를 처리하는 과정에서 query를 보내면 이에 따른 정보를 DB로부터 가져온다.

- DB용 언어를 [[SQL]]이라고 하는데, query는 DB에서 원하는 조건에 맞는 데이터를 조작할 수 있는 SQL 문장의 집합을 말하며, 질의문이라 한다.

- 따라서 query == 질의문 == SQL 다 같은 말로 정의할 수 있다.
## 명령문과 차이점

- 명령문이라고도 부르는 경우가 있지만 디테일하게 따지고 본다면 명령문과는 다르다 볼 수 있다. 
- 그렇기 때문에 명령문이 아닌 질의문이라고 부르는 것이 보다 정확하다고 볼 수 있다.

- 명령문은 이에 따른 실행/취소/에러를 보내는 개념이라면, 질의문은 DB에서 거절하는 것이 가능하다고 한다.

```
ex) ~에 대한 권한이 없습니다.
```

## query 접근법

### 1. 매개변수 제시

- DB에서 사용자가 사용할 수 있는 매개변수목록을 제시해준다.  
- 이미 사용할 목록이 제시되어 있어 간단하지만 대신 유연성이 떨어지는 방법이기도 하다.

## 2. QBE

- `query by example`의 약자로 예제를 통해 쿼리를 제시한다는 것이다. 
- 이 방법은 비어있는 [[레코드(Record)]]를 보여주고, 사용자가 비어있는 [[Spring/SQL/필드(Field)|필드(Field)]]값을 채워 query를 정의토록 한다.

### 3. query언어 사용

- 이 방법은 사용자가 언어를 직접 사용하여 요청을 하는 방식이다. 
- 사용자가 언어를 배워야한다는 어려움이 있지만 유연성이 높다고 할 수 있다.

- 그리고 이러한 방법은 같은 결과에 대해 다양한 방법으로 접근하는 것이 가능하다.  
- 그렇기 때문에 어떻게 접근하느냐에 따라 속도와 효율의 차이가 벌어지기 때문에 이 또한 개인의 능력이 된다.

