- [[Context API]]의 Context [[객체(Object)]] 안에는 Provider라는 [[컴포넌트(Component)]]가 들어있다. 

- 그리고, 그 컴포넌트간에 공유하고자 하는 값을 [[value]]라는 [[props]]로 설정하면 자식 [[컴포넌트(Component)]]들에서 해당 값에 바로 접근을 할 수 있다.


## 문법

```jsx
function App() {
	return (
		<MyContext.Provider value="Hello World">
			<GrandParent />
		</MyContext.Provider>
	);
}
```

- 위처럼 설정하면 원하는 [[컴포넌트(Component)]]에서 [[Context.Consumer]] 혹은 [[useContext()]]라는 [[Hooks]]을 사용하여 Context에 넣은 값에 바로 접근할 수 있다. 