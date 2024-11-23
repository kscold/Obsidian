- useMemo를 사용하면 [[컴포넌트(Component)]] 내부에서 발생하는 연산을 최적화할 수 있다.

- useMemo는 값([[리터럴(literal)]])을 메모이제이션하고 연산 결과를 반환한다.
- useMemo는 계산 비용이 큰 연산을 최적화하는 데 사용한다.
- 예를 들어, [[배열(Array)]]이나 [[객체(Object)]]와 같은 큰 데이터를 가공하거나 복잡한 계산을 수행하는 경우에 사용된다.

- [[useCallback()]]함수와 큰 차이는 useMemo는 useMemo는 계산 비용이 큰 연산의 결과를 메모이제이션하여 재사용하고, useCallback은 함수를 메모이제이션하여 재사용하는 것에 차이가 있다.


## 문법

```jsx
const value = useMemo(() => {
    return calculate();
},[item])
```

- useMemo는 [[useEffect()]]처럼 첫 번째 인자로 [[콜백 함수(Callback Function)]], 두 번째 인자로 의존성 배열(dependancyArray)을 받는다.
- 의존성 배열 안에있는 값이 업데이트 될 때에만 [[콜백 함수(Callback Function)]]를 다시 호출하여 메모리에 저장된 값을 업데이트 해준다.

- 만약 빈 배열을 넣는다면 [[useEffect()]]와 마찬가지로 마운트 될 때에만 값을 계산하고 그 이후론 계속 memoization된 값을 꺼내와 사용한다.


## useMemo를 사용하는 이유

```jsx
import React, { useState, useCallback, useMemo } from "react";

export default function App() {
	const [ex, setEx] = useState(0);
	const [why, setWhy] = useState(0);
	
	// 버튼 클릭시 ex값이 출력된다.
	console.log(ex); 
	
	// useMemo 사용하기 
	useMemo(() => {console.log(ex)}, [ex]);
	  
	return (
		<>
			<button onClick={() => setEx((curr) => (curr + 1))}>X</button>
			<button onClick={() => setWhy((curr2) => (curr2 + 1))}>Y</button>
		</>
	);
}
```

- [[컴포넌트(Component)]]의 [[state]] 값이 변하면 리렌더링이 일어난다.

- 버튼을 클릭할 경우 [[setState]]로 인해 [[state]]값이 변화되어 리렌더링이 일어나면 console.log가 찍히도록 위처럼 코드를 작성하면 된다.
- 사실, 위처럼 코드를 작성할 경우 X버튼, Y버튼, 혹은 컴포넌트가 부모 컴포넌트에 의해서 리렌더링이 될 경우 상태 값에 관계 없이 console.log가 찍히는 연산이 발생하게 된다.
- 만약, 단순하게 console.log가 아닌 복잡한 연산, 연산할 때마다 약 0.3초씩 걸린다고 가정하면 사이트가 굉장히 느려진다.
- 따라서 ex값이 변할 경우에만 연산을 실행할 수 있도록 useMemo를 사용해 ex라는 변수에 의존하도록 등록하는 것이다.

- 결과, 리렌더링이 발생할 경우, 특정 변수가 변할 때에만 useMemo에 등록한 함수가 실행되도록 처리하면 불필요한 연산을 하지 않게 된다.

## useMemo를 사용해야되는 예시

- 밑의 코드에서 useMemo()를 사용하여 getAverage 함수의 중복호출을 막는다.

```jsx
import { useState } from 'react';

const getAverage = (numbers) => {
	console.log('평균값 계산 중...');
	if (numbers.length === 0) return 0;
	
	const sum = numbers.reduce((a, b) => a + b); // 전부 더한 값을 구함
	return sum / numbers.length; // numbers 요소의 갯수로 나눔
};

const Average = () => {
	const [list, setList] = useState([]);
	const [number, setNumber] = useState('');
	
	const onChange = (e) => {
		setNumber(e.target.value);
	};
	
	const onInsert = (e) => {
		const nextList = list.concat(parseInt(number)); // 숫자로 변환하여 리스트에 반영
		console.log(nextList);
		setList(nextList);
		setNumber('');
	};
	
	return (
		<div>
			<input value={number} onChange={onChange} />
			<button onClick={onInsert}>등록</button>
			<ul>
				{list.map((value, index) => (
					<li key={index}>{value}</li>
				))}
			</ul>
			<div>
				<b>평균값:</b> {getAverage(list)}
			</div>
		</div>
	);
};

export default Average;
```

- 위의 코드에서 인풋창의 number 바뀌면 getAverage 함수가 계속 호출되는 이유는 자바스크립트에서 [[객체(Object)]]는 [[원시 타입(Primitive type)]]이 아니라 [[참조 타입(Reference Type)]]이기 주소 값으로 저장되기 때문에 number [[state]]가 바뀌면 [[컴포넌트(Component)]]가 재호출되면서 getAverage 함수도 재호출 되었다고 인식한다.

```jsx
	const avg = useMemo(() => getAverage(list), [list]);
	// 따라서 인풋을 list를 의존성 배열에 넣어 list 값이 업데이트 될 때에만 콜백함수를 호출

	return (
		<div>
			<input value={number} onChange={onChange} />
			<button onClick={onInsert}>등록</button>
			<ul>
				{list.map((value, index) => (
					<li key={index}>{value}</li>
				))}
			</ul>
			<div>
				<b>평균값:</b> {avg} {/* useMemo된 값을 전달 */}
			</div>
		</div>
	);
```


## useMemo()와 [[React.memo()]]의 공통점과 차이점

### 공통점

- [[React.memo()]]와 useMemo() 모두 [[props]]가 변하지 않으면(이전 props와 동일하면) 인자로 넘긴 함수는 재실행되지 않고, 이전의 메모이즈된 결과를 반환한다는 점에서 공통점이있다. 

- 아래 React.memo와 useMemo를 사용한 코드를 보면 두가지 코드는 props.name의 값이 변하지 않는다면 [[리렌더링(Re-rendering)]] 되지 않고 이전의 값을 반환한다는 점에서 동일하게 동작한다.

```jsx
// 별도로 두번째 인자를 넘기지 않을 경우 props가 변하지 않는다면 재렌더링 되지 않음
const NameTag = React.memo(
	(props) => <div>{props.name}</div>
);

// 만약 두번째 인자로 특정 props.name값이 같지 않을때만 재렌더링 하도록 커스텀 비교 함수를 넣어주고 싶을 때
const NameTag = React.memo(
	(props) => <div>{props.name}</div>,
	(prevProps, nextProps) => prevProps.name === nextProps.name
)
```

```jsx
function NameTag(props) {
	return useMemo(() => <div>{props.name}</div>, [props.name])
}
```

### 차이점

- [[React.memo()]]는 [[HOC(Higher-Order-Components)]], useMemo는 [[Hooks]]이다.
- [[React.memo()]]는 [[HOC(Higher-Order-Components)]]이기 때문에 [[클래스형 컴포넌트(Class Component)]], [[함수형 컴포넌트(Functional Component)]] 모두 사용 가능하지만, useMemo는 [[Hooks]]이기 때문에 오직 [[함수형 컴포넌트(Functional Component)]] 안에서만 사용 가능하다.