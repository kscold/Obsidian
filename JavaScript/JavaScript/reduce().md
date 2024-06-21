- 자바스크립트의 reduce함수는 [[배열(Array)]]의 각 요소를 순회하며 [[콜백 함수(Callback Function)]]의 실행 값을 누적하여 하나의 결과값을 반환한다.
- 새로운 [[배열(Array)]]을 반환하므로 [[불변성 유지]]가 된다.

- 파이썬의 sum이랑 비슷하다고 생각하면 된다.
- 비슷하게 reduceRight() 함수를 사용해서 오른쪽부터 계산시킬 수 있다.


## 문법

- 다음 4가지의 [[매개변수(parameter)]]를 가진다.

```js
배열.reduce.(funtcion (accumulator, currentValue, currentIndex, array) {
	return accumulator + currentValue;
})
```
### accumulator 

- accumulator는 [[콜백 함수(Callback Function)]]의 값을 반환값을 누적한다.
### currentValue

- [[배열(Array)]]의 현재 요소이다.
### index(Optional) 

  - [[배열(Array)]]의 현재 요소의 인덱스이다.
### array(Optional) 

-  호출한 [[배열(Array)]]이다.


## reduce() [[메서드(Method)]]의 동작과정

```jsx
var array = [1, 2, 3, 4, 5];
array.reduce(function(prev, cur) {
  return prev + cur;
}); // 15
```

- 배열을 왼쪽부터 조건을 적용해 하나의 값으로 만든다.
- 위의 코드의 조건은 이전 값과 현재 값을 더한 값을 반환것이다.
- 과정은 왼쪽 두 개부터 시작한다.
- 이전 값은 1이고 현재 값은 2라서 더하면 3이 되고, 그 값이 다시 이전 값이 된다. 
- 이제 이전 값은 3이고 현재 값은 3이라서 더하면 6이되고, 다음에는 이전 값이 6이고, 현재 값은 4가 되어서 더하면 10, 마지막으로 이전 값이 10이고 현재 값이 5라서 더하면 15가 되고, 최종 결과를 반환한다.
- 간단히 말하면 조건에 따라, 1 + 2 + 3 + 4 + 5를 한 것이다. 
- 오른쪽부터 줄여가고 싶으면 reduceRight 함수를 사용하면 된다.


## 예시

- 밑의 코드는 [[배열(Array)]]의 모든 값 (1부터 10까지) 더하는 예시이다.

```js
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

const sum1 = numbers.reduce((accumulator, currentNumber) => accumulator + currentNumber);

console.log('sum1 =', sum1); // 55
```

- [[콜백 함수(Callback Function)]] 선언 후 이용하는 예시이다.

```js

function sumReducer(accumulator, currentNumber) {
	return accumulator + currentNumber;
}

const sum2 = numbers.reduce(sumReducer);

console.log('sum2 =', sum2); // 55
```