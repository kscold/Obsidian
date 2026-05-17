- [[Context API]]의 useContext() [[Hooks]]의 인자에는 [[createContext()]]로 만든 context [[객체(Object)]]를 넣는다.


## 사용법

```jsx
import { createContext, useContext } from 'react';

function Message() {
	const value = useContext(MyContext);
	
	return <div>Received: {value}</div>;
}
```

- 위처럼 설정하면 중간 중간 여러 [[컴포넌트(Component)]]를 거쳐 전달하던 [[props]]를 지워주어도 된다.

```jsx
import { createContext, useContext } from 'react';
const MyContext = createContext();

function App() {
	return (
		<MyContext.Provider value="Hello World">
			<GrandParent />
		</MyContext.Provider>
	);
}

function GrandParent() {
	return <Parent />;
}

function Parent() {
	return <Child />;
}

function Child() {
	return <GrandChild />;
}

function GrandChild() {
	return <Message />;
}

function Message() {
	const value = useContext(MyContext);
	return <div>Received: {value}</div>;
}

export default App;
```

- 위의 코드에서 보면 가장 마지막에 존재하는 Message [[컴포넌트(Component)]] useContext를 사용하여 특정 [[state]]를 가지고 올 수 있다.