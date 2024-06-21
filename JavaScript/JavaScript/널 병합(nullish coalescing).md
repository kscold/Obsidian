- 널 병합 연산자는 주로 `||` 연산자 대용으로 사용되며, falsy 값(0, '', false, NaN, null, [[undefined]]) 중 null과 [[undefined]]만 따로 구분한다.

- 널 병합 연산자는 `??`으로 사용한다.

- 실제 자바스크립트는 널 병합 연산자보다 [[옵셔널 체이닝(Optional Chaning)]]이 더 많이 쓰인다.

## `||`와 `??` 연산자의 차이

```js
const a = 0;
const b = a || 3; // || 연산자는 falsy한 값이면 뒤의 값이 할당됨
console.log(b); // 3

const c = 0;
const d = c ?? 3; // ?? 연산자는 null과 undefined일 때만 뒤의 값이 할당됨
console.log(d); // 0 falsy한 값이므로 0이 대입됨

// null과 undefined인 경우는 3이 대입됨
const e = null;
const f = e ?? 3;
console.log(f); // 3 

const g = undefined;
const h = g ?? 3;
console.log(h); // 3
```