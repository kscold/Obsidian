- [[배열(Array)]] [[객체(Object)]]의 내장 함수인 map 함수는 반복되는 [[컴포넌트(component)]]를 렌더링할 수 있다.
- map를 사용해서 배열 내 각 요소를 원하는 규칙에 따라 변환 후, 그 결과로 새로운 배열을 생성한다.


## 문법

- 첫번째 매개변수는 [[콜백 함수(callback)]]으로 새로운 배열의 요소를 생성하는 함수로 파라미터는 다음 세가지이다.
	- currentValue: 현재 처리하고 있는 요소이다.
	- index: 현재 처리하고 있는 요소의 index 값이다.
	- array: 현재 처리하고 있는 원본 배열이다.
- thisArg(선택 항복): callback 함수 내부에서 사용할 [[this]] 레퍼런스

```jsx
arr.map(callback, [thisArg])
```

## map 함수의 예시

```js
var numbers = [1, 2, 3, 4, 5]

var processed = numbers.map(function(num){
	return num * num;
})

console.log(processed);

// >> [1, 4, 9, 16, 25]
```

- 밑의 코드는 위의 코드를 es6 문법으러 작성한 코드이다.

```js
const numbers = [1, 2, 3, 4, 5]
const result = numbers.map(num => num * num); // 화살표 함수도 사용 가능
console.log(result);

// >> [1, 4, 9, 16, 25]
```