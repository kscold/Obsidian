- undefined는 자바스크립트에서 제공하는 [[원시 타입(Primitive type)]]의 값이다.
- 리액트 컴포넌트에서는 함수의 undefined만 반환하여 렌더링하는 상황을 만들면 안된다.

## 예시

- 밑의 코드를 작성하면 오류가 난다.

```jsx
import App from './App.css';

function App(){
	const name = undefined;
	
	return name;
}

export default App;
```

- 따라서 이렇게 OR 연산자를 이용하여 예외처리를 하든지 [[JSX]]에 넣어 버리면 된다.

```jsx
import App from './App.css';

function App(){
	const name = undefined;
	return name || "값이 undefined입니다.";
	
	// 또는
	const name = undefined;
	return <div>{name}</div>
	
	// undefined 일때 보여주고 싶은 문구가 있을 때
	const name = undefined;
	return <div>{name || '리액트'}</div>
}

export default App;
```