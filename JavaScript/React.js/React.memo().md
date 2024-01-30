- [[컴포넌트(component)]]의 [[리렌더링(Re-rendering)]]을 방지할 때는 [[shouldComponentUpdate()]]라는 [[생명 주기(Life Cycle)]] [[메서드(Method)]]를 이용하면 되나, [[함수형 컴포넌트(Functional Component)]]에서는 라이프사이클 메서드를 이용할 수 없다.

- 따라서 React.memo라는 [[함수(Function)]]를 사용하여 [[컴포넌트(component)]]의 [[props]]가 바뀌지 않았으면, [[리렌더링(Re-rendering)]]하지 않도록 설정하여 함수 컴포넌트의 리렌더링 성능을 최적화해줄 수 있다.

- 리액트의 [[불변성 유지]]가 되지 않으면 [[객체(Object)]] 내부의 값이 새로워져도 바뀐 것을 감지하지 못하기 때문에 React.memo에서 서로 비교하여 최적화하는 것이 불가능하다.

## 예시

```jsx
import {
	MdCheckBoxOutlineBlank,
	MdCheckBox,
	MdRemoveCircleOutline,
} from 'react-icons/md';
import './TodoListItem.scss';
import React from 'react';

const TodoListItem = ({ todo, onRemove, onToggle }) => {
	const { id, text, checked } = todo;
	
	return (
		<div className="TodoListItem">
			<div
				className={`checkbox ${checked ? 'checked' : ''}`}
				onClick={() => onToggle(id)}
			>
				{checked ? <MdCheckBox /> : <MdCheckBoxOutlineBlank />}
				<div className="text">{text}</div>
			</div>
			
			<div className="remove" onClick={() => onRemove(id)}>
				<MdRemoveCircleOutline />
			</div>
		</div>
	);
};

export default React.memo(TodoListItem);
```