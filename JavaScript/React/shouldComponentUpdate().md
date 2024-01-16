- [[props]] 또는 [[state]]를 변경했을 때, 리렌더링을 시작할지 여부를 지정하는 [[메서드(Method)]]이다.
- 이 [[메서드(Method)]]에서는 반드시 true 값 또는 false 값을 변환해야 한다.
- [[컴포넌트(component)]]를 만들 때 이 메서드를 따로 생성하지 않으면 기본적으로 언제나 true 값을 반환한다.
- 이 메서드가 false값을 반환한다면 업데이트 과정은 여기서 중지된다.


- 이 메서드 안에서 현재 [[props]]와 [[state]]는 [[this]].props와 this.state로 접근하고, 새로 설정될 props 또는 state는 nextProps nextState로 접근할 수 있다.

- 프로젝트 성능을 최적화할 때, 가장 중요하게 사용된다.
- 따라서 상황에 맞는 알고리즘을 작성해야 하며, 리렌더링 방지할 때는 false 값을 반환하게 만든다.

## 매개변수

- nextProps
	- 첫번째 매개변수로 현재 props의 값이다.
- nextState
	- 첫번째 매개변수로 현재 state의 값이다.

```jsx
shouldComponentUpdate(nextProps, nextState) {
	// 컴포넌트가 리렌더링을 해야 할지 말아야 할지를 결정하는 메서드
	console.log('shouldComponentUpdate', nextProps, nextState);
	// 현재 컴포넌트 state의 number 숫자의 마지막 자리가 4면 리렌더링하지 않음
	return nextState.number % 10 !== 4;
}
```