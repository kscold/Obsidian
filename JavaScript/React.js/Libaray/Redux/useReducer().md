- useReducer()는 [[useState()]]보다 더 다양한 [[컴포넌트(component)]] 상황에 따라 다양한 상태([[state]])를 다른 값으로 업데이트해 주고 싶을 때 사용하는 [[Hooks]]이다.
- 리액트의 [[리덕스(Redux)]]의 동작과정에 포함된다.

## 문법

- 첫번째 매개변수는 [[reduce()]] 함수를 넣다.
- 두번째 매개변수는 리듀서의 기본값([[state]]의 초기값)을 넣어 준다.
- 세번째 매개변수에 초기 상태를 만들어주는 함수을 넣어주면 컴포넌트가 렌더링될 때만 그 함수가 호출된다.
- 이 [[Hooks]]를 사용하면 [[state]]값과 [[dispatch()]] 함수를 받아 온다.

```jsx
function reducer(state, action) { // 리듀서를 선언
	switch (action.type) { // action의 type
		case 'INCREMENT':
			return { value: state.value + 1 };
		case 'DECREMENT':
			return { value: state.value - 1 };
		default: // else와 같은 역할
			return state;
	}
}
	
const Counter = () => {
	const [state, dispatch] = useReducer(reducer, { value: 0 });
	
	return (
		<div>
			<p>
				현재 카운터 값은 <b>{state.value}</b>입니다.
			</p>
			<button onClick={() => dispatch({ type: 'INCREMENT' })}>+1</button>
			<button onClick={() => dispatch({ type: 'DECREMENT' })}>-1</button>
		</div>
	);
};
```