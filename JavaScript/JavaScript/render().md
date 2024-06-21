- render 함수는 [[생명 주기(Life Cycle)]] 중에서 [[컴포넌트(Component)]]의 [[생성자(Constructor)]] 과정에 속한다.
- [[생명 주기(Life Cycle)]] [[메서드(Method)]] 중 유일한 필수 메서드이다.

- 이 메서드 안에서 [[this]].[[props]]와 [[this]].[[state]]에 접근할 수 있으며, 리액트 요소를 반환한다.
- 요소는 [[<div>]] 같은 태그가 될 수도 있고, 따로 선언한 컴포넌트가 될 수도 있다.
- 아무것도 보여 주고 싶지 않다면 null 값이나 false 값을 반환하도록 한다.

- render() 는 return 되는 html 형식의 코드를 화면에 그려주는 함수이다.
- 화면 내용이 변경되어야 할 시점에 자동으로 호출된다.

- 주의사항으로는 이 메서드 안에서는 [[이벤트(event)]] 설정이 아닌 곳에서 [[setState]]를 사용하면 안되고, 브라우저의 [[DOM(Document Object Model)]]에 접근해서도 안되며 DOM 정보를 가져오거나 state에 변화를 줄 때는 componentDidMount에서 처리해야 한다.

## 예시

- 밑에 코드는 LifecycleEx 라는 컴포넌트를 생성한 후 App.js에서 임포트해 사용해보자.

```jsx
import React, {Component} from "react";

class LifecyecleEx extends Componemt {
	render() {
		console.log('render call!');
		return(
			<h2>This is render function</h2>
		)
	}
}
```

- 밑에 코드는 부모컴포넌트인 App.js 설정이다.

```jsx
import './App.css';
import { React } from 'react';
import LifecycleEx from './LifecycledEx';

fuction App() {
	return (
		<div>
			<h1>Start React 200!</h1>
			<p>css 적용하기</p>
			<LifecycleEx></LifecycleEx>
		</div>
	);
}

export default App;
