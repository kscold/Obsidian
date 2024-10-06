- 커링(Currying)은 [[함수(Function)]]와 함께 사용하는 고급기술이다.
- 커링은 [[함수(Function)]]를 호출하는 것이 아닌 변환하는 것이다.

- 이 기술은 오직 자바스크립트에만 존재하는 것이 아닌 다른 언어에도 존재한다.


## 커링(Currying)의 의미

- f(a, b, c)처럼 단일 호출로 처리하는 함수를 f(a)(b)(c)와 같이 각각의 [[매개변수(parameter)]]로 호출 가능한 프로세스로 호출된 후 병합될 수 있게 변환하는 것이다.


## 커링 함수 예시

- 아래 예시는 동적이지 않은 커링 함수로 변환하는 코드이다.

```js
function log(date, importance, message) {
	alert(
		`[${date.getHours()}: ${date.getMinutes()}]: [${importance}], ${message}`
	);
}


// 동적이지 않은 커링 함수 변환
function curry(f) {
	return function (a) {
		return function (b) {
			return function (c) {
				return f(a, b, c); // 하나의 함수로 합침
			};
		};
	};
}

const curriedLog = curry(log); // 일반함수를 커링함수로 변경함
curriedLog(new Date())('DEBUG')('some bug'); // 정상 실행
```

- 아래 예시는 동적으로 변환되는 커링 함수 코드이다.

```js
// 인자 갯수에 상관 없이 동적으로 커링 함수로 변환
function curry(func) {
	return function curried(...args) {
		if (args.length >= func.length) {
			return func.apply(this, args);
		} else {
			return function (...args2) {
				return curried.apply(this, args.concat(args2));
			};
		}
	};
}

const sum = (x, y, z, j, a) => x + y + z + j + a;
const curriedSum = curry(sum);

console.log(curriedSum(1)(2)(3)(4)(5)); // >> 15
```