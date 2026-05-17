- react-virtualized를 사용하면 리스트 컴포넌트에서 스크롤되기 전에 보이지 않는 컴포넌트는 렌더링하지 않고 크기만 차지하게끔 할 수 있다.
- 스크롤이 될 시에 해당 스크롤 위치에서 보여 주어야 할 컴포넌트를 자연스럽게 렌더링시키는 것이다.
- 이 라이브러리를 사용하면 낭비되는 자원을 아주 쉽게 아낄 수 있다.

## 예시

```jsx
import React, { useCallback } from 'react';
import TodoListItem from './TodoListItem';
import './TodoList.scss';
import { List } from 'react-virtualized';

const TodoList = ({ todos, onRemove, onToggle }) => {
	const rowRenderer = useCallback( // Renderer 함수를 선언
		({ index, key, style }) => { // 리스트 요소 비구조화 할당
			const todo = todos[index];
			return (
				<TodoListItem
					todo={todo}
					key={key}
					onRemove={onRemove}
					onToggle={onToggle}
					style={style}
				/>
			);
		},
	[onRemove, onToggle, todos]);
	
	return (
		<List
			className="TodoList"
			width={512} // 전체 크기
			height={513} // 전체 높이
			rowCount={todos.length} // 항목 갯수
			rowHeight={57} // 항목 높이
			rowRenderer={rowRenderer} // 항목을 렌더링할 때 쓰는 함수(Renderer 함수 연결)
			list={todos} // 배열
			style={{ outline: 'none' }} // List에 기본 적용되는 outline 스타일 제거
		/>
	);
};  

export default React.memo(TodoList);
```

- 이후 기존의 보여주던 내용을 div로 한번 더 감싸고, 해당 div에는 TodoListItem-virtualized라는 className을 설정하고, props로 받아온 style을 적용시켜 준다.

```jsx
import {
	MdCheckBoxOutlineBlank,
	MdCheckBox,
	MdRemoveCircleOutline,
} from 'react-icons/md';
import './TodoListItem.scss';
import React from 'react';


const TodoListItem = ({ todo, onRemove, onToggle, style }) => { 
	// props로 받아온 style을 적용
	const { id, text, checked } = todo;
	
	return (
		<div className="TodoListItem-virtualized" style={style}>
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
		</div>
	);
};

export default React.memo(TodoListItem);
```