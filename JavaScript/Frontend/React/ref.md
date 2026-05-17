- 리액트 [[컴포넌트(Component)]] 안에도 id를 사용할 수 있으나, 같은 [[컴포넌트(Component)]]를 재사용하는 방법에 있어, [[DOM(Document Object Model)]]에서 id는 unique(유일)해야 하는데 이런 상황에서 중복 id를 가진 DOM이 여러 개 생기니 잘못된 사용이 된다.


## ref를 만드는 방법

- ref를 만드는 방법은 크게 3가지가 있다.
	1. ref는 [[콜백 함수(Callback Function)]]를 통해 만들 수 있다.
	2. createRef를 통해 ref를 설정할 수 있다.([[클래스형 컴포넌트(Class Component)]])
	3. [[useRef()]]를 사용하여 ref를 설정할 수 있다.([[함수형 컴포넌트(Functional Component)]])

- 밑에 코드는 콜백 함수로 만드는 방법이다.

```jsx
<input ref={(ref) => {this.input=ref}}>
```

- createRef를 사용해서 만드는 방법이다.
```jsx
import React, { Component } from 'react'

class RefSample extends Component {
	input = React.createRef();

	handleFocus = () => {
		this.input.current.focus(); // 이 focus() 메서드를 통해 input창에 커서를 유지한다.
	}

	render() {
		return{
			<div>
				<input ref={this.input} />
			</div>
		};
	}
}

export default RefSample;
```

- current.focus() [[메서드(Method)]]를 사용해서 접근할 수 있다.
- 이렇게 ref를 설정해 준 DOM에 접근하려면 this.input.current를 조회하면 된다.

## 컴포넌트에 ref 달기
- 리액트에서는 컴포넌트에도 ref를 달 수 있다.
- 이 방법은 컴포넌트 내부에 있는 DOM을 컴포넌트 외부에서 사용할 때 쓴다.
- 컴포넌트에 ref를 다는 방법은 DOM에 ref를 다는 방법과 똑같다.

```jsx
<MyComponent 
	ref={(ref) => {thsi.myComponent=ref}}
/>			
```

- 위와 같은 형식을 사용하여 내부의 ref도 접근할 수 있다.

## 바닐라 [[HTML(Hyper Text Markup Language)]]에서의 id 사용법

```html
<html lang="en">

<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>Example</title>
	<style>
		.success {
			background-color: green;
		}
		.failure {
			background-color: red
		}
	</style>
	<script>
		function vaildate() {
			var input = document.getElementById('password')
			input.className = '';
			
			if (input.value === '0000') {
				input.className = 'success'
			} else {
				input.className = 'failure'
			}
		}
	</script>
</head>

<body>
	<input type="password" id="password"></input>
	<button onclick="vaildate()">Vaildate</button>
</body>

</html>
```