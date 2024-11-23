- 이 [[메서드(Method)]]는 [[JavaScript/render()]]에서 만들어진 결과물이 브라우저에 실제로 반영되기 직전에 호출된다.
- [[DOM(Document Object Model)]]에 변화가 일어나기 직전의 색상 속성을 snapshot 값으로 반환하여 이것을 [[componentDidupdate()]]에서 조회할 수 있다.
- 이 메서드에서 반환하는 값은 [[componentDidupdate()]]에서 세 번째 파라미터인 snapshot 값으로 전달받을 수 있다.
- 주로 업데이트하기 직전의 값을 참고할 일이 있을 때 활용된다.(예) 스크롤바 위치 유지)

```js
getSnapshotBeforeUpdate(prevProps, preState) {
	if(preState.array !== this.state.array) {
		const { scrollTop, scrollHeight } = this.list
		return { scrollTop, scrollHeight };
	}
}
```