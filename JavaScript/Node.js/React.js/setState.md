- 리액트 [[클래스형 컴포넌트(Class Component)]]에서 [[useState()]]([[함수형 컴포넌트(Functional Component)]]) 대신 사용되는 예약어로 [[this]]를 통해 접근한다.
- useStaet에서 사실 setter 함수 작동 방식은 setState와 같다.
- this.state는 여러 값을 넣을 수 있다.
- this.setState를 사용하여 state값을 업데이트할 때는 상태가 [[비동기(asynchronous)]]적으로 업데이트 된다.

```jsx
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

### this.state에서 this 생략하기
- 클래스 컴포넌트에서 this를 생략해서 바로 state로 접근할 수 있다.

```jsx
import React, { Component } from "react";

class Counter extends Component {
	// state의 초깃값 설정하기
	state = {
		number: 0,
		fixedNumber: 0,
	};
  
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


### this.setState에 [[객체(Object)]] 대신 함수 인자 전달하기

```jsx
onClick={() => {
	// this.setState를 사용하여 state에 새로운 값을 넣을 수 있다.
	this.setState({ number: number + 1 });
	this.setState({ number: this.state.number + 1 });
}}
```

- 위와같이 작성하면 this.setState를 두 번 사용하는 것임에도 불구하고 버튼을 클릭할 때 숫자가 1씩 더해진다.
- 따라서 이에 대한 해결책은 this.setState를 사용할 때 객체 대신 함수를 인자로 넣어주는 것이다.


```js
this.setState((prevState, props)) => {
	return (
		// 업데이트하고 싶은 내용
	)
}
```


### PrevState
- 이때 prevState는 기존 생타이고, [[props]]는 현재 지니고 있는 props를 가리킨다.

```jsx
import React, { Component } from "react";

class Counter extends Component {
	// state의 초깃값 설정하기
	state = {
		number: 0,
		fixedNumber: 0,
	};
  
	render() {
		const { number, fixedNumber } = this.state;
		return (
			<div>
				<h1>{number}</h1>
				<h2>바뀌지 않는 값: {fixedNumber}</h2>
				<button onClick={() => {
					this.setState((prevState) => {
						return {
							number: number + 1,
						};
					}); // this.setState를 사용하여 state에 새로운 값을 넣을 수 있다.
				}}>
					+1
				</button>
			</div>
		);
	}
}

export default Counter;
```

```jsx
this.setState((prevState) => ({ // 이때 () 소괄호는 return을 포함한다.
	number: number + 1, 
}));
```

- [[화살표 함수(Arrow function)]]에서 값을 바로 반환하고 싶다면 코드 블록 { }를 생략하면 된다.

## 클래스 컴포넌트에서 input 여러 개 다루기(onChange)
- input이 여러 개일 때는 event 객체를 활용하면 훨씬 편하다.
- event 객체 중에 event.target.name 값을 사용하여 유동적으로 input을 다룬다. onChange 이벤트 핸들러에서 event.target.name은 해당 인풋의 name을 가리킨다.

```jsx
import { Component } from 'react';

class EventPractice extends Component {
	state = {
		username: '',
		message: '',
	};

	  
	handleChange = (e) => {
		// 화살표함수로 선언하면 메서드 바인딩을 생략할 수 있다.
		this.setState({
			[e.target.name]: e.target.value,
		});
	};
	
	  
	
	handleClick = () => {
		alert(this.state.username + ': ' + this.state.message);
		this.setState({
			username: '',
			message: '',
		});
	};

	render() {
		return (
			<div>
				<h1>이벤트 연습</h1>
				<input
					type="text"
					name="username"
					placeholder="사용자명"
					value={this.state.username}
					onChange={this.handleChange}
				/>
				<input
					type="text"
					name="message"
					placeholder="아무거나 입력해 보세요"
					value={this.state.message}
					onChange={this.handleChange}
				/>
				<button onClick={this.handleClick}>확인</button>
			</div>
		);
	}
}

export default EventPractice;

```

- 이 코드에서 핵심은 [[객체(Object)]] 안에서 key를 `[ ]`로 감싸면 그 안에 넣은 레퍼런스가 가리키는 실제 값(value)가 key 값으로 사용되는 점을 이용한다.

## 특정 키 값으로 메서드 실행하기
- onKeyPress 객체에 메서드를 넘기면 실행가능하다.

```jsx

handleKeyPress = (e) => { // event를 받아서
	if (e.key === 'Enter') { // e.key로 특정키 여기서는 Enter를 눌렀을 때
		this.handleClick(); // 동작을 실행
	}
};

// 이후 리턴 부에
<input
	type="text"
	name="message"
	placeholder="아무거나 입력해 보세요"
	value={this.state.message}
	onChange={this.handleChange}
	onKeyPress={this.handleKeyPress}
/> // onKeyPress를 넘겨줌

```
