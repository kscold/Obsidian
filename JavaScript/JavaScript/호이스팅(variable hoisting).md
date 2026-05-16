- Hoist의 뜻은 무언가를 들어 끌어올리는 동작을 의미한다.
- 변수 선언문이 코드의 선두로 끌어 올려진 것처럼 동작하는 자바스크립트 고유의 특징을 호이스팅이라고 한다.

- 변수 뿐만아니라 [[var]], [[let]], [[const]], [[function]], function*, [[클래스(class)]] [[키워드(Keyword)]]를 사용해서 선언하는 모든 [[JavaScript/식별자(Identifier)]]는 호이스팅 된다.

- 정확히 말하면 호이스팅은 "선언이 코드 위로 이동"하는 것이 아니라, [[소스코드의 평가 과정]]에서 자바스크립트 엔진이 코드를 한 줄씩 실행하기 **전에** [[렉시컬 환경(Lexical Environment)]]에 [[JavaScript/식별자(Identifier)]]를 미리 등록해두기 때문에 일어나는 현상이다.

- 모든 선언문은 [[런타임(runtime)]] 이전 단계에서 먼저 처리되기 때문이다.


## 선언 키워드별 호이스팅 동작

| 키워드 | 호이스팅 여부 | 선언 전 접근 시 |
|---|---|---|
| [[var]] | O | `undefined` ([[변수(Variable)]] 자동 초기화) |
| [[let]] | O | `ReferenceError` (TDZ에 갇힘) |
| [[const]] | O | `ReferenceError` (TDZ에 갇힘) |
| [[function]] 선언식 | O | 정상 호출 가능(함수 전체가 등록) |
| function 표현식 | 변수 규칙을 따름 | `var`면 `undefined`, `let`/`const`면 `ReferenceError` |
| [[클래스(class)]] | O | `ReferenceError` (TDZ에 갇힘) |

- [[var]]는 [[렉시컬 환경(Lexical Environment)]]에 키만 만들고 `undefined`로 자동 초기화한다 → 선언 줄 이전에 참조해도 에러 없이 `undefined`를 받는다.

- [[let]], [[const]], [[클래스(class)]]는 등록만 되고 초기화되지 않은 상태(`<uninitialized>`)로 시작한다 → 선언 줄 이전에 참조하면 `ReferenceError`가 발생하며, 이 구간을 **TDZ(Temporal Dead Zone, 일시적 사각지대)** 라고 한다.

- [[함수(Function)]] 선언식은 등록 단계에서 **함수 본체까지 통째로** 메모리에 올라가기 때문에 선언 줄 이전에도 호출이 가능하다.

```js
console.log(a); // undefined  ← var는 호이스팅 + undefined 초기화
var a = 1;

console.log(b); // ReferenceError ← let은 TDZ
let b = 1;

sum(1, 2); // 3 ← 함수 선언식은 본체까지 호이스팅
function sum(x, y) { return x + y; }

bar(); // TypeError: bar is not a function ← var bar는 undefined인 상태
var bar = function () { return 5; };
```


## 함수 표현식은 변수 선언 규칙을 따른다

- 함수 표현식은 함수가 변수에 **대입되는 형태**이므로, 그 변수의 선언 키워드([[var]]/[[let]]/[[const]])의 호이스팅 규칙을 그대로 따른다.
- 즉, 선언 줄 이전에는 함수가 아직 변수에 대입되지 않았으므로 호출하면 에러가 난다.
