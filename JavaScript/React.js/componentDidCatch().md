- [[생명 주기(Life Cycle)]] [[메서드(Method)]]이다.
- componentDidCatch, [[컴포넌트(component)]] 렌더링 도중에 에러가 발생했을 때 애플리케이션이 먹통이 되지 않고 오류 UI를 보여 줄 수 있게 해준다.

```jsx
import { Component } from 'react';
  
class ErrorBoundary extends Component {
	state = {
		error: false,
	};
	componentDidCatch(error, info) {
		this.setState({ // state에 error를 true로 설정
			error: true
		});
		console.log({ error, info })
	}
	
	render() {
		if (this.state.error) { // error가 true 이면
			return <div>에러가 발생했습니다!</div>; // 문구를 렌더링
		}
		
		return this.props.children; // error가 false이면 children 컴포넌트를 렌더링
	}
}

export default ErrorBoundary;
```

- 첫번째 매개변수 error는 어떤 에러가 발생했는지 알려 준다.
- 두번째 매개변수 info 어디에 있는 코드에서 오류가 발생했는지에 대한 정보를 준다.

- 앞의 코드에서 그저 console.log만 했지만, 나중에 실제로 사용할 때 오류가 발생하면 서버 API를 호풀하여 따로 수질할 수 있다.

- 이 메서드를 사용할 때는 컴포넌트 자신에게 자신에게 발생하는 에러를 잡아낼 수 없고, 자신의 [[this]].[[props]].[[children]]으로 전달되는 컴포넌트에서 발생하는 에러만 잡아낼 수 있다는 점을 알아 두어야 한다.
