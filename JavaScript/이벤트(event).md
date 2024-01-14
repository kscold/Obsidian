- 사용자가 웹 르바우저에서 DOM 요소들과 상호작용하는 것을 event라고 한다.

- [[HTML]]에서 [[onMouseOver]], [[onClick]], [[onChange]] 이벤트를 실행한다.

- 바닐라 [[HTML]]에서는 카멜케이스를 사용하지 않는다.

## 문법

### 바닐라
- onclick=""
- 이벤트에 실행할 자바스크립트 코드를 ""안에 전달한다.

### [[JSX]](리액트)
- onClick={}
- 이벤트에 실행할 함수 형태의 값을 전달한다.
- 리액트에서는 함수 형태의 [[객체(Object)]]를 전달한다.(보통 [[화살표함수(Arriw Funtion)]]로 만들어서 전달한다.)
- DOM 요소에만 이벤트를 설정 할 수 있다.(즉 div, button, input, form, span 등의 DOM 요소에는 이벤트를 설정할 수 있지만 우리가 직전 만든 [[컴포넌트(component)]]에는 이벤트를 자체적으로 설정할 수 없다.

```jsx
<MyComponent onClick={doSomething}>
```

- 사실 위의 예시에서 처럼 MyComponent에 onClick 값을 설정한다면 MyComponent를 클릭 할 때, doSomething 함수를 실행하는 것이 아니라, 그냥 이름이 onClick인 [[props]]를 MyComponent에 전달해 줄 뿐이다.
- 따라서 [[컴포넌트(component)]] 자체적으로 이벤트를 설정할 수는 없다. 하지만 밑의 예시처럼 전달받은 [[props]]를 컴포넌트 내부의 [[DOM(Document Object Model)]] 이벤트로 설정할 수는 있다.

```jsx
<div onClick={this.props.onClick}> {/* 현재 onClick 객체를 가르킨다. */}
	{/* (...) */}
</div>
```

- 이런식으로 [[onChange]]
```jsx
import { Component } from 'react';

  
class EventPractice extends Component {
	render() {
		return (
			<div>
				<h1>이벤트 연습</h1>
				<input
					type="text"
					name="message"
					placeholder="아무거나 입력해 보세요"
					onChange={(e) => {
						console.log(e.target.value);
					}}
				/>
			</div>
		);
	}
}

export default EventPractice;
```


## event.target.value
- [[onChange]] 같은 이벤트가 발생할 때, 값이 바뀔 때 마다 e.target.value가 들어간다.

### [[state]]에 input 값 담기
- 