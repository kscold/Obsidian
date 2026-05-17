- [[생명 주기(Life Cycle)]] [[메서드(Method)]]로 props로 받아온 값을 state에 동화시키는 용도 사용하면, [[컴포넌트(Component)]]가 마운트될 때 업데이트될 때 호출된다.

```js
static getDerivedStateFromProps(nextProps, prevState) {
	if(nextProps.value !== prevState.value) { // 조건에 따라 특정 값 동기화
		return { value: nextProps.value };
	}
	return null; // state를 변경할 필요가 없다면 null를 반환
}
```

