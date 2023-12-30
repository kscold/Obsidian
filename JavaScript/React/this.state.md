- 리액트 [[클래스형 컴포넌트]]에서 [[useState]] 대신 사용되는 예약어로 [[this]]를 통해 접근한다.
- this.state는 여러 값을 넣을 수 있다.

```js
import React, { Component } from "react";

class Counter extends Component {

constructor(props) {
	super(props); // props를 오버라이딩
	// state의 초깃값 설정하기
	this.state = {
		number: 0,
		fixedNumber: 0,
	};
}

render() {
	const { number, fixedNumber } = this.state;
	
	return (
		<div>
			<h1>{number}</h1>
			<h2>바뀌지 않는 값: {fixedNumber}</h2>
			<button onClick={() => {
				this.setState({ number: number + 1 }); 
				// this.setState를 사용하여 state에 새로운 값을 넣을 수 있다.
				}}>
				+1
			</button>
		</div>
		);
	}
}

export default Counter;
```