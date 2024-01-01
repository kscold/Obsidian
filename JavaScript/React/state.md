- [[클래스형 컴포넌트]]든 [[함수형 컴포넌트]]든 state를 사용하여 값을 바꾼다.
- state값을 바꾸어야 할 때는 [[setState]] 혹은 [[useState]]를 통해 전달받은 세터 함수(set이름)를 사용해야한다.

### [[배열]]이나 [[객체(Object)]]를 업데이트 할 때
- 이런 상황에서는 배열이나 객체 사본을 만들고 그 사본에 값을 업데이트한 후, 그 사본의 상태를 setState 혹은 세터 함수를 통해 업데이트한다.

```js
// 객체 다루기
const object = { a: 1, b: 2, c: 3}
const nextObject = {...object, b: 2}; // 사본을 만들어서 b 갑만 덮어 쓰기

// 배열 다루기
const array = [
	{ id: 1, value: true },
	{ id: 2, value: true },
	{ id: 3, value: false },
];

let nextArray = array.concat({ id: 4 }); // 새 항목 추가
nextArray.filter(item => item.id !== 2); // id가 2인 항목 제거
nextArray.map(item => (item.id === 1 ? { ...item, value: false} : item); 
// id가 1인 항목의 value를 false로 설정

```

- 객체에  [[확산 연산자(spread)]]라고 불리는 ...을 이용하여 처리하고, 배열에 대한 사본을 만들 때는 배욜의 내장 함수들을 이용하여 활용한다.

