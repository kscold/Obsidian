- [[리액트(React)]]에서 함수형 컴포넌트(Functional Component)는 [[클래스형 컴포넌트(Class Component)]]와는 다른 구문을 사용하여 작성되는 컴포넌트이다.

- 주로 간단한 상태([[state]]나 [[생명 주기(Life Cycle)]] [[메서드(Method)]]가 필요하지 않은 경우에 사용된다.

- 함수형 컴포넌트는 주로 React 버전 16.8부터 도입된 [[Hooks]]라는 개념과 함께 사용된다. 
- Hooks는 함수형 컴포넌트에서도 상태(state)와 생명 주기 메서드와 같은 기능을 사용할 수 있도록 해준다.

## 문법

```jsx
import React from 'react';

// 함수형 컴포넌트 정의

const MyFunctionalComponent = () => {
	return (
		<div>
		    <h1>Hello, I'm a functional component!</h1>
	    </div>
	);
};

export default MyFunctionalComponent;
```