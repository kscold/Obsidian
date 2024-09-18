- [[타입스크립트(TypeScript)]]에서 declare는 컴파일러에게 declare 로 선언된 [[변수(Variable)]] 또는 [[함수(Function)]]들을 이미 존재한다고 알리는 것이다.

- declare로 재정의 된 변수에 [[타입 표기(Type Annotation)]]도 가능하다.
- declare [[키워드(Keyword)]]는 변환된 index.js에서는 나타나지않는다.


## 예시

```js
// js파일에 변수를 선언
var num = 10;	
var profile = {name: 'kim'};
```

- 다른 js파일에 선언된 num 변수를 [[타입스크립트(TypeScript)]]에서 declare로 불러와 사용할 수 있다.

```ts
declare let num: number; // declare로 변수가 이미 존재한다는것을 알림
console.log(num + 1);
```

