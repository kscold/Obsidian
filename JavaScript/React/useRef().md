- useRef는 [[함수형 컴포넌트]]에서 [[ref]]를 쉽게 사용할 수 있도록 만들어 준다.


## 예시
- 지역 [[변수(Variable)]]를 만들 때도 useRef를 사용할 수 있다.

- [[클래스형 컴포넌트]]에서 로컬 변수를 사용하는 예시이다.
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

-  함수형 컴포넌트로 작성한 예시

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


