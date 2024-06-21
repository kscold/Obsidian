- undefined는 자바스크립트에서 제공하는 [[원시 타입(Primitive type)]]의 값이다.
- [[null]]도 [[원시 타입(Primitive type)]]이다.
- undefined 타입은 undefined 값이 유일하며 [[null]] 타입은 [[null]] 값이 유일하다.

- undefined는 아무 값도 할당받지 않은 상태를 의미한다.
- [[var]] [[키워드(Keyword)]]로 선언한 [[변수(Variable)]]는 [[호이스팅(variable hoisting)]]으로 올라간 후 [[undefined]]로 초기화 된다.
- 그 이후 인터프리터가 해당 소스코드에 도달했을 때 할당란 값이 들어가게 된다.

- 즉, undefined는 개발자가 의도적으로 할당하기 위한 값이 아닌 자바스크립트 엔진이 [[변수(Variable)]]를 초기화할 때 사용한다.
- 따라서 개발자가 의도적으로 [[undefined]]를 할당하는 것은 권장되지 않는다.

- [[리액트(React)]] [[컴포넌트(Component)]]에서는 [[함수(Function)]]의 undefined만 반환하여 렌더링하는 상황을 만들면 안된다.


## 예시

- 밑의 코드를 작성하면 오류가 난다.

```jsx
import App from './App.css';

function App(){
	const name = undefined;
	
	return name;
}

export default App;
```

- 따라서 이렇게 OR 연산자를 이용하여 예외처리를 하든지 [[JSX]]에 넣어 버리면 된다.

```jsx
import App from './App.css';

function App(){
	const name = undefined;
	return name || "값이 undefined입니다.";
	
	// 또는
	const name = undefined;
	return <div>{name}</div>
	
	// undefined 일때 보여주고 싶은 문구가 있을 때
	const name = undefined;
	return <div>{name || '리액트'}</div>
}

export default App;
```