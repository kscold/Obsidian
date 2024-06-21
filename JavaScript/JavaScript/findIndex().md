- ES6부터 findIndex()라는 새로운 [[메서드(Method)]]를 Array.[[prototype]]에 추가했다.

- findIndex() 메서드는 테스트 기능을 충족하는 요소의 인덱스를 반환하거나 테스트를 통과한 요소가 없으면 -1을 반환한다.
- 이를 통해 제공된 테스트 기능을 충족하는 [[배열(Array)]]의 첫 번째 요소(item)를 찾을 수 있다.

## 문법

- findIndex()는 두 개의 인자를 취한다.

```js
findIndex(testFn(element[, index[, array]])[, thisArg])
```
### testFn

- testFn은 요소가 발견되었음을 나타내는 true를 반환할 때까지 [[배열(Array)]]의 각 요소에 대해 실행하는 [[함수(Function)]]이다.
- 즉, [[콜백 함수(Callback Function)]]로도 사용가능하다.

- testFn은 세 가지 인자를 사용한다.
#### element
-  처리 중인 [[배열(Array)]]의 현재 요소(item)이다.
#### index
- index는 처리 중인 현재 요소의 인덱스이다.
#### array
- findIndex()가 호출된 [[배열(Array)]]이다.
### thisArg

- thisArg는 [[콜백 함수(Callback Function)]]을 실행할 때 사용할 선택적 [[객체(Object)]]이다.
- thisArg 인자를 생략하면 findIndex() 함수는 [[undefined]]를 사용한다.

- findIndex()는 testFn이 true로 강제 변환되는 값인 truthy 값을 반환하는 항목을 찾을 때까지 [[배열(Array)]]의 모든 요소에 대해 testFn을 실행한다.
- findIndex()가 그러한 요소를 찾으면 즉시 요소의 인덱스를 반환한다.


## 예시

### 간단한 배열 예제와 함께 배열 findIndex() 메서드 사용

- 밑에 예시는 ranks 배열에서 숫자 7이 처음 나타나는 인덱스를 반환한다.

```js
let ranks = [1, 5, 7, 8, 10, 7]; 
let index = ranks.findIndex(rank => rank === 7);

console.log(index);

// >> 2
```

### 보다 복잡한 조건에서 Array findIndex() 메서드 사용

- 밑의 예시에서는 findIndex() 메서드를 사용하여 순위 배열에서 인덱스 2 다음에 숫자 7이 처음 나타나는 인덱스를 가져온다.

```js
let ranks = [1, 5, 7, 8, 10, 7]; 
let index = ranks.findIndex((rank, index) => rank === 7 && index > 2 );

console.log(index);
// >> 5
```

### 객체 배열과 함께 Array findIndex() 메서드 사용

- 밑에 예시에서는 Array findIndex() 메서드를 사용하여 가격이 1000보다 큰 첫 번째 제품의 인덱스를 찾는다.

```js
const products = [  
	{ name: 'Phone', price: 999 },   
	{ name: 'Computer', price: 1999 },   
	{ name: 'Tablet', price: 995 }, 
];  

const index = products.findIndex(product => product.price > 1000);  

console.log(index); 

// >> 1
```