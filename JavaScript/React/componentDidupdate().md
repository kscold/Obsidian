- 리렌더링을 완료한 후 실행된다.
- 업데이트가 끝난 직후이므로 [[DOM(Document Object Model)]]관련 처리를 해도 무방하다.
- 여기서는 prevProps 또는 prevState를 사용해서 [[컴포넌트(component)]]가 이전에 가졌던 데이터에 접근할 수 있다.

- 또 [[getSnapshotBeforeUpdate()]]에서 반환한 값이 있다면 여기서 snapshot 값을 전달받을 수 있다.


```jsx
componentDidUpdate(prevProps, prevState) {
	if (prevProps.value !== this.props.value) {
		doSomething();
	}
}
```

## 매개변수
- prevProps
	- 상속받은 [[props]]의 이전 상태
- prevState
	- 현재 [[state]]의 이전 상태 