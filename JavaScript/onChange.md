- 바닐라 html에서는 [[키워드(Keyword)]]가 onchange이다.
- Form 요소 값이 바뀔 때 onChange 이벤트를 실행한다.

- onChange의 value에 [[화살표 함수(Arriw Funtion)]]로 [[이벤트(event)]]를 [[변수(Variable)]]로 받으면 그 변수에 이벤트 [[객체(Object)]]가 들어간다.

## [[클래스형 컴포넌트]]의 event 담기

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

- [[SyntheticEvent]]는 네이티브 이벤트와 달리 이벤트가 끝나고 이벤트가 초기화되므로 정보를 참조할 수 없다.
- 예를 들어, 0.5초 뒤에 e 객체를 참조하면 e 객체 내부의 모든 값이 비워지게 된다.
- 따라서 만약 비동기적으로 이벤트 객체를 참조할 일이 있다면 e.persist()함수를 호출해 주어야 한다.

```jsx
import { Component } from 'react';

  
class EventPractice extends Component {

	state = {
		message: '',
	};


	render() {
		return (
			<div>
				<h1>이벤트 연습</h1>
				<input
					type="text"
					name="message"
					placeholder="아무거나 입력해 보세요"
					value={this.state.message}
					onChange={
						(e) => {
							this.setState({
								message: e.target.value,
							});
						}
					}
				/>

				<button
					onClick={
						() => {
							alert(this.state.message);
							this.setState({ message: '' });
						}
					}
				>
				확인
				</button>
			</div>
		);
	}
}

export default EventPractice;
```

- 위의 예시는 클래스형 컴포넌트에서 event.target.value사용하여 [[state]]에 텍스트 담는 예시이다.
- 코드에서처럼 onChange [[onClick]]같은 속성은 리액트에서는 이벤트에 실행할 자바스크립트 코드를 전달하는 것이 아니라, 함수 형태의 값을 전달한다는 것 을 알 수 있다.

- 그러나 이 방법 대신 함수를 미리 준비하여 전달하는 것이 가독성 측면에서 더 좋다.
- 