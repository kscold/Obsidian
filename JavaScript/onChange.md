- 바닐라 html에서는 [[키워드(Keyword)]]가 onchange이다.
- Form 요소 값이 바뀔 때 onChange 이벤트를 실행한다.

- onChange의 value에 [[화살표함수(Arriw Funtion)]]로 [[이벤트(event)]]를 [[변수(Variable)]]로 받으면 그 변수에 이벤트 [[객체(Object)]]가 들어간다.

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

- 이 때 이벤트 객체 e는 [[SyntheticEvent]]로 웹 브라우저의 네이티브 이벤트를 감싸는 객체이다. 네이티브 이벤트와 인터페이스가 같으므로 순수 자바스크립트에서 [[HTML]] 이벤트를 다둘 때와 똑같이 사용하면 된다.
