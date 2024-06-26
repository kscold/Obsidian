- state(상태)는 [[컴포넌트(Component)]] 안에서 관리되는 것이다.
- [[클래스형 컴포넌트(Class Component)]]든 [[함수형 컴포넌트(Functional Component)]]든 state를 사용하여 값을 바꾼다.

- 즉, [[컴포넌트(Component)]]의 렌더링 결과물에 영향을 주는 데이터를 가지고 있는 [[객체(Object)]]이다.

- state값을 바꾸어야 할 때는 [[setState]] 혹은 [[useState()]]를 통해 전달받은 세터 함수(set이름)를 사용해야 한다.
- state는 바뀔 때마다 [[불변성 유지]]가 된다.


## Component간의 정보 공유

- 자식 [[컴포넌트(Component)]]들 간의 다이렉트 데이터 전달은 불가능하다.
- 자식 [[컴포넌트(Component)]]들 간의 데이터를 주고 받을 때는 상태를 관리하는 부모 [[컴포넌트(Component)]]를 통해서 주고 받는다.

- 그런데 자식이 많아진다면 상태 관리가 매우 복잡해진다.
- 상태를 관리하는 상위 컴포넌트에서 **계속 내려 받아**야한다. ([[Props Drilling]] 이슈가 발생한다.)

[![리액트-상태관리-복잡성](https://i0.wp.com/hanamon.kr/wp-content/uploads/2021/07/%E1%84%85%E1%85%B5%E1%84%8B%E1%85%A2%E1%86%A8%E1%84%90%E1%85%B3-%E1%84%89%E1%85%A1%E1%86%BC%E1%84%90%E1%85%A2%E1%84%80%E1%85%AA%E1%86%AB%E1%84%85%E1%85%B5-%E1%84%87%E1%85%A9%E1%86%A8%E1%84%8C%E1%85%A1%E1%86%B8%E1%84%89%E1%85%A5%E1%86%BC.jpg?resize=300%2C477&ssl=1)](https://hanamon.kr/codestates-til-%ed%95%ad%ed%95%b4%ec%9d%bc%ec%a7%80-44%ec%9d%bc%ec%b0%a8/%e1%84%85%e1%85%b5%e1%84%8b%e1%85%a2%e1%86%a8%e1%84%90%e1%85%b3-%e1%84%89%e1%85%a1%e1%86%bc%e1%84%90%e1%85%a2%e1%84%80%e1%85%aa%e1%86%ab%e1%84%85%e1%85%b5-%e1%84%87%e1%85%a9%e1%86%a8%e1%84%8c%e1%85%a1/)

## 상태 관리의 복잡성을 해결해주는 라이브러리를 활용

- 상태 관리 툴은 가장 크게 2가지 문제를 해결해준다.
	1. 전역 상태 저장소 제공한다.
	2. Props drilling 이슈 해결해준다.

- 예를 들어, `<A>`라는 컴포넌트에 상태가 있고, `<I>`라는 컴포넌트가 해당 상태를 사용한다고 하면,
- 그 중간에 존재하는 `<C>`, `<G>` 등은 굳이 name이라는 상태가 필요하지 않음에도, 컴포넌트에 [[props]]를 만들어 자식 컴포넌트에 넘겨주어야 했다.
- 이를 props drilling(프로퍼티 내려꽂기) 문제라고 부른다.
- 전역 상태 저장소가 있고, 어디서든 해당 저장소에 접근할 수 있다면 이러한 문제는 해결 된다.

### 상태 관리 툴 종류
- React Context
- [[리덕스(Redux)]]
- MobX

### [[배열(Array)]]이나 [[객체(Object)]]를 업데이트 할 때

- 이런 상황에서는 [[배열(Array)]]이나 [[객체(Object)]] 사본을 만들고 그 사본에 값을 업데이트한 후, 그 사본의 상태를 setState 혹은 setter 함수를 통해 업데이트한다.

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

- [[객체(Object)]]에  [[확산 연산자(spread operator)]]라고 불리는 ...을 이용하여 처리하고, [[배열(Array)]]에 대한 사본을 만들 때는 [[배열(Array)]]의 내장 함수들을 이용하여 활용한다.

