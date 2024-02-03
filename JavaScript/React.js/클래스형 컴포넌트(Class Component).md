

## 클래스 컴포넌트의 사용 방식

- 클래스 컴포넌트에서 props를 사용할기 위해서는 아래의 방식을 따라야 한다.

	1. [[constructor()]] [[키워드(Keyword)]]를 통해 [[props]]를 생성한다.
	2. [[super()]] 키워드를 통해 props를 오버라이딩 한다. 혹은 바로 state에 접근하여 1 2 번 과정을 생략한다.
	3. [[setState]] [[메서드(Method)]]를 사용해서 [[객체(Object)]]형식으로 key:value를 정의한다.
	4. 이후 [[JavaScript/render()]] 키워드를 사용해서 화면에 렌더링하는 부분을 정의한다.(return 부를 포함한다.)

- 클래스형 컴포넌트는 render() 함수를 사용하여 렌더링을 지정해주어야 한다.

```jsx
import React, { Component } from 'react';

class App extends Component {
	render() {
		const name = "react";
		return <div className="react">{name}</div>;
	}
}
```

```jsx
import React, { Component } from 'react';

class Dog{ // 클래스 정의

	constructor(name) {
		this.name = name; // 생성자 정의
	}

	say() {
		console.log(this.name + ': 멍멍');
	}
}

const dog = new Dog('흰둥이');
dog.say(); // 흰둥이: 멍멍

```

## 클래스형 컴포넌트에서 props 사용하기

- 클래스형 컴포넌트에서 props를 사용할 때는 render 함수에서 this.props를 조회하면 된다.
- defaultProps와 propTypes는 똑같은 방식으로 설정할 수 있다.

```jsx
import React, { Component } from 'react';
import PropTypes from 'prop-types';

class MyComponent extends Component {
	render() {
		const { name, faveriteNumber, children } = this.props; // 비구조화 할당

		return (
			<div>
				안녕하세요, 제 이름은 {name}입니다. <br />
				children 값은 {children}
				입니다.
				<br />
				제가 좋아하는 숫자는 {favoriteNumber}입니다.
			</div>
		);
	}
}

MyComponent.defaultProps = {
	name: '기본 이름';
};

MyComponent.propTypes = {
	name: PropTypes.string;
	favoriteNumber: PropTypes.number.isRequired
};

export default MyComponent;
```

- class 내부에서 지정하는 방법도 있다.
```jsx
import React, { Component } from 'react';
import PropTypes from 'prop-types';

class MyComponent extends Component {
	static defaultProps = {
		name: '기본 이름'
	}

	static propTypes = {
		name: PropTypes.string,
		averiteNumber: PropTypes.number.isRequired
	}

	render() {
		const { name, faveriteNumber, children } = this.props; // 비구조화 할당

		return (
			<div>
				안녕하세요, 제 이름은 {name}입니다. <br />
				children 값은 {children}
				입니다.
				<br />
				제가 좋아하는 숫자는 {favoriteNumber}입니다.
			</div>
		);
	}
}

export default MyComponent;
```

## 클래스형 컴포넌트의 state

- [[super()]]를 이용한다.

```jsx
import React, { Component } from 'react';

class Counter extends Component {
	constructor(props) {
		super(props); // 미리 짜여져있는 props 메소드를 오버라이딩
		// state의 초깃값 설정하기
		this.state = {
			number: 0
		};
	}

	render() {
		const { number } = this.state; // state를 조회할 때는 this.state로 조회한다.
		return (
			<div>
				<h1>{number}</h1>
				<button 
					// onClick을 통해 버튼이 클릭되었을 때 호출할 함수를 지정한다.
					onClick{() => {
						// this.setState를 사용하여 state에 새로운 값을 넣을 수 있다.
						this.setState({ number: number + 1});
					}}
				>
					+1
				</button>
			</div>
		);
	}
}

export default Counter;
```

- state 객체 안에 여러 값이 있을 때
```jsx
import React, { Component } from 'react';

class Counter extends Component {
	constructor(props) {
		super(props); // 미리 짜여져있는 props 메소드를 오버라이딩
		// state의 초깃값 설정하기
		this.state = {
			number: 0
			fixedNumber: 0
		};
	}

	render() {
		const { number, fixedNumber } = this.state; // state를 조회할 때는 this.state로 조회한다.
		return (
			<div>
				<h1>{number}</h1>
				<h1>바뀌지 않는 값: {fixedNumber}</h1>
				<button 
					// onClick을 통해 버튼이 클릭되었을 때 호출할 함수를 지정한다.
					onClick{() => {
						// this.setState를 사용하여 state에 새로운 값을 넣을 수 있다.
						this.setState({ number: number + 1});
					}}
				>
					+1
				</button>
			</div>
		);
	}
}

export default Counter;
```