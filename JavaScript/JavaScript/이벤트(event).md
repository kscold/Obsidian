- 사용자가 웹 브라우저에서 [[DOM(Document Object Model)]] 요소들과 상호작용하는 것을 event라고 한다.

- [[HTML(Hyper Text Markup Language)]]에서 [[onMouseOver]], [[onClick]], [[onChange]] 이벤트를 실행한다.

- 바닐라 [[HTML(Hyper Text Markup Language)]]에서는 카멜케이스를 사용하지 않는다.


## 문법

### 바닐라 자바스크립트

- onclick="" 형식으로 사용한다.
- 이벤트에 실행할 자바스크립트 코드를 ""안에 전달한다.
- 바닐라 HTML에서 [[DOM(Document Object Model)]] 요소의 이름을 달 때에는 [[id]]를 사용한다.

### [[JSX]](리액트)

- onClick={} 형식으로 사용한다.
- 리액트의 이벤트 시스템은 웹 브라우저의 HTML 이벤트와 인터페이스가 동일하기 때문에 사용법이 거의 비슷하다.([[JSX]] 이용한다.)

- 이벤트에 실행할 함수 형태의 값을 전달한다.
- 리액트에서는 함수 형태의 [[객체(Object)]]를 전달한다.(보통 [[화살표 함수(Arrow function)]]로 만들어서 전달한다.)

- DOM 요소에만 이벤트를 설정 할 수 있다.(즉 div, button, input, form, span 등의 DOM 요소에는 이벤트를 설정할 수 있지만 우리가 직전 만든 [[컴포넌트(Component)]]에는 이벤트를 자체적으로 설정할 수 없다.

```jsx
<MyComponent onClick={doSomething}>
```

- 사실 위의 예시에서 처럼 MyComponent에 [[onClick]] 값을 설정한다면 MyComponent를 클릭 할 때, doSomething 함수를 실행하는 것이 아니라, 그냥 이름이 onClick인 [[props]]를 MyComponent에 전달해 줄 뿐이다.
- 따라서 [[컴포넌트(Component)]] 자체적으로 이벤트를 설정할 수는 없다. 

- 하지만 밑의 예시처럼 전달받은 [[props]]를 컴포넌트 내부의 [[DOM(Document Object Model)]] 이벤트로 설정할 수는 있다.

```jsx
<div onClick={this.props.onClick}> {/* 현재 onClick 객체를 가르킴 */}
	{/* (...) */}
</div>
```


## event.target.value

- [[onChange]] 같은 이벤트가 발생할 때, 값이 바뀔 때 마다 e.target.value가 들어간다.



## 바닐라 자바스크립트에서 이벤트

- 바닐라 자바스크립트에서 이벤트를 등록하는 방법은 크게 3가지가 있다.

### 자바스크립트 코드에서 프로터피로 등록

```js
// 문서가 load 될 때 이 함수를 실행
window.onload = function () {
	// 아이디가 "text"인 요소를 반환함
	let text = document.getElementById("text");
	
	text.innerHTML = "HTML 문서 loaded"
}
```

### [[HTML(Hyper Text Markup Language)]] 태그에 속성으로 등록

- [[이벤트(event)]]를 사용할때 [[onClick]]의 바닐라 버전은 아래와 같이 onclick = "" 로 사용한다.

```html
<button onclick="alert('버튼이 클릭됐습니다.')">Click</button>
```


### [[addEventListener()]] [[메서드(Method)]]를 사용

```js
const aElement = document.querySelector('a');
aElement.addEventListener('click', () => {
	alert('a element clicked')
})
```



## [[JSX]]에서 이벤트

- 밑의 예시는 [[클래스형 컴포넌트(Class Component)]]의 이벤트 예시이다.
- [[<input>]] 태그에서는 [[onChange]]을 사용하여 이벤트를 받는다.

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

- 밑의 코드는 [[함수형 컴포넌트(Functional Component)]]의 이벤트 예시이다.

```jsx
import React, { useState } from 'react';

const Say = () => {
	const [message, setMessage] = useState('');
	const onClikEnter = () => setMessage('안녕하세요!');
	const onClikLeave = () => setMessage('안녕히 가세요!');
	
	const [color, setColor] = useState('black');
	
	return (
		<div>
			<button onClick={onClikEnter}>입장</button>
			<button onClick={onClikLeave}>퇴장</button>
		</div>
	)
};

export default Say;
```


## [[노드(Node.js)]]에서 이벤트

- [[노드(Node.js)]]에서 [[이벤트(event)]]는 [[비동기(asynchronous)]]적인 프로그래밍에서 중요한 개념 중 하나이다.
- 이벤트는 특정한 동작이나 상황을 나타내며, 이러한 이벤트가 발생했을 때 그에 따른 동작을 실행할 수 있다.

- 이벤트를 처리하기 위해서는 이벤트를 발생시키는(emit) 측과 그 이벤트를 감지하고 처리하는(listener) 측이 있다.

### [[이벤트 에밋(Event Emit)]]

- 이벤트를 발생시키는 것을 의미한다.
- 특정한 상황이나 조건이 충족되었을 때 이벤트를 발생시켜 다른 부분에 알리는 역할을 한다.

- 주로 `emit()` 메서드를 사용하여 이벤트를 발생시킨다.
- 이 [[메서드(Method)]]를 호출하면 해당 이벤트가 발생하고, 등록된 리스너들이 이를 감지할 수 있다.

### [[이벤트 리스너(Event Listener)]]

- 이벤트를 감지하고 그에 따른 동작을 정의하는 역할을 한다.
- 즉, 특정한 이벤트가 발생했을 때 실행될 콜백 함수를 정의한다.

- [[on()]] 메서드를 사용하여 특정 이벤트에 대한 리스너를 등록할 수 있다.
- 이 때, 해당 이벤트가 발생하면 등록된 [[콜백 함수(Callback Function)]]가 실행된다.

### 이벤트 사용 예시

-  아래 코드는 `myEmitter`라는 이벤트 발생기를 생성하고, 이벤트인 `customEvent`를 발생시키고 이를 감지하는 리스너를 등록하는 예시이다.

```js
const EventEmitter = require('events');  // 이벤트 발생기 생성 
const myEmitter = new EventEmitter();  // 이벤트 리스너 등록 

myEmitter.on('customEvent', () => {   
	console.log('Custom event occurred'); 
}); // 이벤트 발생

myEmitter.emit('customEvent');
```
`

- 위 코드에서 `emit()`은 [[이벤트(event)]]를 발생시키고, [[on()]]은 [[이벤트 리스너(Event Listener)]]를 등록한다.
- 따라서 `customEvent`가 발생하면 이를 감지하여 "Custom event occurred"를 출력한다.