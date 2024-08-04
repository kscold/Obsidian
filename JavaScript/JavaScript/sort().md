- 자바스크립트에서 [[배열(Array)]]을 정렬하기 위해서는 sort() [[메서드(Method)]]를 사용한다.

- 기본으로 오름차순으로 정렬하지만, 숫자 등 다른 데이터 타입을 정렬하려면 [[콜백 함수(Callback Function)]]를 사용해서 원하는 정렬 순서로 변경할 수 있다.


## 문법

```js
arr.sort([compareFunction])
```

- compareFunction 규칙에 따라서 정렬된 배열을 반환한다.
- 이때, 원본 배열인 arr가 정렬이 되고, 반환하는 값 또한 원본 배열인 arr을 가리키고 있음에 유의해야 한다.

### compareFunction

- 정렬 순서를 정의하는 [[함수(Function)]]이다.
- 이 값이 생략되면, 배열의 element들은 문자열로 취급되어, 유니코드 값 순서대로 오름차순으로 정렬된다.

- 이 함수는 두 개의 배열 element를 파라미터로 입력 받는다.
- 이 함수가 a, b 두개의 element를 파라미터로 입력받을 경우, 이 함수가 리턴하는 값이 0보다 작을 경우,  a가 b보다 앞에 오도록 정렬하고, 이 함수가 리턴하는 값이 0보다 클 경우, b가 a보다 앞에 오도록 정렬한다.

- 만약 0을 리턴하면, a와 b의 순서를 변경하지 않는다.

```js
const numbers = [1, 1000, 10, 31];

function compareNumbers(a, b) {
	return a - b;
}

numbers.sort(compareNumbers);

console.log(numbers); // 출력: [1, 10, 31, 1000]
```






