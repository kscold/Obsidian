- 사용자가 웹 브라우저에서 [[DOM(Document Object Model)]] 요소들과 상호작용하는 것을 event라고 한다.

- [[HTML(Hyper Text Markup Language)]]에서 [[onMouseOver]], [[onClick]], [[onChange]] 이벤트를 실행한다.

- 바닐라 [[HTML(Hyper Text Markup Language)]]에서는 카멜케이스를 사용하지 않는다.


## 문법

### 바닐라

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
