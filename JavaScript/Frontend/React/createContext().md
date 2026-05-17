- [[Context API]]에서 Context는 [[리액트(React)]] 패키지에서 createContext()라는 [[메서드(Method)]]를 불러와서 만들 수 있다.


## 문법 

```jsx
import { createContext } from 'react';

const MyContext = createContext();
```

- 위의 형식처럼 간단하게 createContext()를 만들어도 되고, 아래 형식처럼 [[Custom Hooks]]와 같이 초기 [[값(value)]]이나 [[객체(Object)]]를 넣어서 정의한 다음 [[컴포넌트(Component)]]화 시켜도 된다.

```jsx
const ColorContext = createContext({
	state: {
		color: 'black',
		subcolor: 'red',
	},
	actions: {
		setColor: () => {},
		setSubcolor: () => {},
	},
});
```

- [[Reducer]] [[함수(Function)]]처럼 [[state]]와 [[action]]을 정의하여 사용하는 방식이다.