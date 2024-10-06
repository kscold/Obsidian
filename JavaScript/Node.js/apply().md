- 프로토타입([[prototype]])의 apply() [[메서드(Method)]]는 주어진 [[this]] 값과 [[배열(Array)]](또는 유사 배열 객체)로 제공되는 arguments로 [[함수(Function)]]를 호출한다.
- 즉. apply() [[메서드(Method)]]를 사용하는 즉시 바로 호출이 된다.

- 이 함수의 구문은 거의 [[call()]] 구문과 유사하다.

- 근본적인 차이점은 [[call()]]은 [[함수(Function)]]에 전달될 인자 리스트를 받는데 비해, apply()는 인자들의 단일 [[배열(Array)]]을 받는다는 점이다.


## 문법

```js
func.apply(thisArg, [argsArray]);
```

### thisArg(옵션)

- func를 호출하는데 제공될 [[this]]의 값이다.
- [[this]]는 [[메서드(Method)]]에 의해 실제로 보여지는 값이 아닐 수 있음을 유의한다.
- [[메서드(Method)]]가 non-strict mode코드의 함수일 경우, null과 [[undefined]]가 [[전역 객체(Global Object)]]로 대체되며, 기본 값은 제한된다.

## argsArray(옵션)

- func이 호출되어야 하는 인자를 지정하는 유사 배열 객체, 함수에 제공된 인수가 없을 경우 또는 [[undefined]]이다.

- ECMAScript 5의 시작으로 이러한 인자들은 [[배열(Array)]] 대신 제네릭 유사 [[배열(Array)]] [[객체(Object)]]로 사용될 수 있다.

### 반환 값

- 지정한 [[this]]값과 인자들로 호출한 [[함수(Function)]]의 결과를 반환한다.