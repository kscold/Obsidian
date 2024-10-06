- `...` [[키워드(Keyword)]]를 사용하여 이전의 데이터를 그대로 복사하여 [[객체(Object)]]나 [[배열(Array)]]의 복사본을 만든다.

- 이때 내부의값을 복사할 때는 [[얕은 복사]]를 하게 된다.
- 즉, 내부의 값이 완전히 새로 복사되는 것이 아니라 가장 바깥쪽에 있는 값만 복사된다.
- 따라서 내부의 값이 [[객체(Object)]] 혹은 [[배열(Array)]]이라면 내부의 값 또한 따로 복사해 주어야 한다.


## 기존 배열을 보존

- [[push()]]와 reverse()의 경우 원본 [[배열(Array)]]의 값을 변환하기 때문에 [[불변성 유지]]가 되지 않는다 따라서 [[concat()]]과 같이 확산 연산자를 사용할 수 있다.

- 아래 코드는 원본 [[배열(Array)]]이 바뀌는 코드이다.

```js
const arr1 = [1, 2, 3];
const arr2 = arr1.reverse();

console.log(arr1); // [3, 2, 1]
console.log(arr2); // [3, 2, 1]
```

- 아래 코드는 확산 연산자를 이용하여 [[불변성 유지]]를 하는 코드이다.

```js
const arr1 = [1, 2, 3];
const arr2 = [...arr1].reverse(); // 새로운 배열에 얕은 복사를 해서 가져옴

console.log(arr1); // [1, 2, 3]
console.log(arr2); // [3, 2, 1]
```

## 배열을 조합

- [[concat()]] [[메서드(Method)]]를 사용해서 [[배열(Array)]]을 합치는 예시이다.

```js
const arr1 = [1, 2, 3]
const arr2 = [1, 2, 3]
const arr3 = [1, 2, 3]
const arrWrap = arr1.concat(arr2, arr3)

console.log(arrWrap);
```

- 아래처럼 확산연사자를 사용해서 [[배열(Array)]]을 합칠 수 있다.

```js
const arr1 = [1, 2, 3]
const arr2 = [1, 2, 3]
const arr3 = [1, 2, 3]
const arrWrap = [arr1, arr2, arr3]

console.log(arrWrap);
```


## 예시

```jsx
const todos = [{ id: 1, checked: true }, { id: 2, checked: true }];
const nextTodos = [...todos];

nextTodos[0].checked = false;
console.log(todos[0] === nextTodos[0]); // 아직까지는 똑같은 객체를 가리키고 있기 때문에 ture

nextTodos[0] = {
	...nextTodos[0],
	checked: false
};
console.log(todos[0] === nextTodos[0]); // 새로운 객체를 할당해 주었기에 false
```

- 만약 [[객체(Object)]] 안에 있는 [[객체(Object)]]라면 [[불변성 유지]]를 하면서 새 값을 할당해야 하므로 다음과 같이 해주어야 한다.

```jsx
const nextComplexObject = {
	...complexObject,
	objectInside: {
		...complexObject.objectInside,
		enabled: false
	}
};

console.log(complexObject === nextComplexObject); // false
console.log(complexObject.objectInside === nextComplexObject.objectInside) // false
```

- 따라서 위의 코드와 같이 배열 혹은 객체의 구조가 복잡해진다면 이렇게 불변성을 유지하면서 업데이트하는 것도 까다로워진다.
- 이렇게 복잡한 상황일 경우 [[immer]]라는 라이브러리의 도움을 받으면 편하다.


## [[리액트(React)]]의 확산 연산자 사용

- [[리액트(React)]]에서는 [[컴포넌트(Component)]] [[props]]에서도 확산연산자가 사용가능하다.

- 아래 코드처럼 상위에서 호출하는 부모 [[컴포넌트(Component)]]에서 title과 children을 제외한 모든 [[props]]를 넘긴다고 가정했을 때, `...` 연산자를 통해 [[props]] [[객체(Object)]]를 가져올 수 있고, [[JSX]]에서 태그의 [[props]]로 또 넘겨줄 수 있다.

- 이 때 주의해야할 점은 ...props의 경우 가장 마지막 [[매개변수(parameter)]]에 위치해야 한다.

```jsx
import React from 'react';

  
const Section = ({ title, children, ...props }) => {
	return (
		<section {...props}>
			<h2>{title}</h2>
			{children}
		</section>
	);
};


export default Section;
```