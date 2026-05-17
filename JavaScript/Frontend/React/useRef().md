- useRef는 [[함수형 컴포넌트(Functional Component)]]에서 [[ref]]를 쉽게 사용할 수 있도록 만들어 준다.
- [[useState()]]가 있는데 useRef를 사용하는 이유는 보통 ref로 설정하는 값들은 렌더링되는 정보가 아니기 때문이다.

## useRef의 사용이유

- 저장공간([[변수(Variable)]] 관리)에 용의하다.
- 저장공간이라 하면 보통 [[state]]가 떠올를 텐데 State의 값을 바꿀 때 대표적으로 [[Hooks]]의 [[useState()]]를 이용한다.
- 리액트 [[컴포넌트(Component)]]는 state가 변할 때마다 다시 렌더링이 되면서 컴포넌트 내부 변수들이 초기화가 된다. 
- 컴포넌트 내부 변수들이 초기화가 된다는 것은 해당 컴포넌트 함수의 변수들이 모두 초기화가 되고 모든 함수 로직 등이 다시 실행되는 것을 의미한다.
- 이렇게 원하지 않는 렌더링 때문에 곤란할 때가 있다.
- 그렇다면 State대신 [[ref]]안에 값을 저장하면 어떨까?
- [[ref]]의 유용한 점은 Ref안에 있는 값을 아무리 변경해도 컴포넌트는 다시 렌더링 되지 않는다.
- 즉, State 대신 Ref를 사용한다면 불필요한 렌더링을 막을 수 있다.
- 또한 [[컴포넌트(Component)]]가 아무리 렌더링이 되어도 Ref안에 저장되어 있는 값은 변화되지 않고 그대로 유지가 된다. 
- 따라서 state를 변경 시 렌더링을 발생시키지 말아야 하는 값을 다룰 때 정말 편리합니다.


## 예시
- 지역 [[변수(Variable)]]를 만들 때도 useRef를 사용할 수 있다.

- [[클래스형 컴포넌트(Class Component)]]에서 로컬 변수를 사용하는 예시이다.
```jsx

import { Component } from 'react';

class RefSampleClass extends Component {
	id = 1;
	setId = (n) => { // 자바의 setter와 비슷
		this.id = n;
	};
	
	printId = () => {
		console.log(this.id);
	};
	
	render() {
		return <div>MyComponent</div>;
	}	
}
```

-  [[함수형 컴포넌트(Functional Component)]]로 작성한 예시이다.

```jsx
import React from 'react';

const RefSampleHooks = () => {
	const id = useRef(1);
	const setId = (n) => {
		id.current = n;
	};
	
	const printId = () => {
		console.log(id.current);
	};
	
	return <div>refsample</div>;
};

export default RefSampleHooks;
```

- 위와 같이 [[ref]] 안의 값이 바뀌어도 [[컴포넌트(Component)]]가 렌더링이 되지 않는다는 점에 주의해야한다.
- 따라서 렌더링과 관련되지 않은 값을 관리할 때만 이러한 방식을 사용해야한다.

- 밑의 코드는 값을 입력하고 [[<input>]] 태그에 ref [[속성(Property)]]을 주어 useRef의 current.focus()를 하는 예시이다.

```jsx
import { useCallback, useMemo, useState, useRef } from 'react';

const getAverage = (numbers) => {
	console.log('평균값 계산 중...');
	if (numbers.length === 0) return 0;
	const sum = numbers.reduce((a, b) => a + b); // 전부 더한 값을 구함
	return sum / numbers.length; // numbers 요소의 갯수로 나눔
};
  
const Average = () => {
	const [list, setList] = useState([]);
	const [number, setNumber] = useState('');
	const inputEl = useRef(null);
	
	const onChange = useCallback((e) => { // 컴포넌트가 처음 랜더링될 때만 함수 생성
		setNumber(e.target.value);
	}, []);
	
	const onInsert = useCallback(() => { // number 혹은 list가 바뀌었을 때만 함수 생성
		const nextList = list.concat(parseInt(number)); // 숫자로 변환하여 리스트에 반영
		console.log(nextList);
		setList(nextList);
		setNumber('');
		inputEl.current.focus(); // useRef를 사용하여 input 창에 focus를 함
	}, [number, list]);
	
	const avg = useMemo(() => getAverage(list), [list]);
	
	return (
		<div>
			<input value={number} onChange={onChange} ref={inputEl} />
			{/* ref를 useRef를 사용하여 focus를 가져옴 */}
			<button onClick={onInsert}>등록</button>
			<ul>
				{list.map((value, index) => (
					<li key={index}>{value}</li>
				))}
			</ul>
			<div>
				<b>평균값:</b> {avg}
			</div>
		</div>
	);
};

export default Average;
```


