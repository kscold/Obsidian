- [[리덕스(Redux)]]의 [[액션 생성함수(Action Creator)]]를 실행하여 [[리덕스(Redux)]] [[스토어(Store)]]에 변경된 상태([[state]])값을 저장하기 위해서는 useDispatch()라는 [[리액트(React)]] [[Hooks]]을 사용하여 [[action]]을 실행시켜야 한다.


## 문법

```jsx
import { useDispatch } from 'react-redux';

cosnt dispatch = useDispatch(); // dispatch로 재선언하여 사용한다.
```

- useDispatch객체를 dispatch로 [[인스턴스(Instance)]]화 한 후, dispatch 변수를 활용하여 액션([[action]])을 호출할 수 있다.


## 예시

- 아래와 같은 [[Reducer]]가 파일이 있다고 가정한다.

```js
// fruit.js
// fruit이라는 기능을 가진 리듀서

// 액션
const SET_FRUIT_LIST = "fruit/SET_FRUIT_LIST";

// 액션 생성 함수
export const setFruitList = fruistList => ({ type : SET_FRUIT_LIST, fruitList });

// 초기값
const initialState = {
	name: false,
	price: false,
};

// 리덕스 스토어값 변경
export default function fruit(state = initialState, action) {
	switch(action.type) {
	    case SET_FRUIT_LIST :
		    return {
		        ...state,
		        name: action.fruitList,
		    };
	    
	    default:
		    return state;
	}
}
```

- 이제 사용하고자 하는 [[컴포넌트(Component)]]에서 dispatch를 사용하여 액션([[action]])을 호출하고 변경할 상태([[state]])값 내용을 넣어 상태값을 변경할 수 있다.

```js
dispatch( setFruitList( "딸기" ) );
```

- 실행할 액션함수명을 적은 후, 해당 액션함수의 파라미터에 변경할 상태값을 추가하고 dispatch로 감싸면 해당 액션을 호출하는 dispatch함수가 완성된다.
- [[매개변수(parameter)]] 값에는 문자열 뿐만 아니라, true / false, 정수, json 타입 등 다양한 타입의 내용을 넣을 수 있다.

```jsx
export const setFruitList = fruistList => ({ type : SET_FRUIT_LIST, fruitList });
```

- 액션 함수의 fruitList에는 위에서 추가한 '딸기'라는 값이 들어가게 된다.

- 아래 코드를 통해 마지막 부분에 스토어값을 변경시키는 함수 fruit부분을 살펴보자. 

- action.fruitList에는 위에서 추가한 '딸기'라는 값이 들어있다.
- 이 딸기라는 값을 name이라는 [[리덕스(Redux)]] [[스토어(Store)]]의 상태([[state]])에 저장한다.

- 즉, 리덕스 스토어 초기값을 살펴보면 name에는 false라는 초기값이 들어가 있다.
- 하지만 dispatch를 통해 setFruitList라는 액션에 '딸기'라는 값을 넣고 액션을 호출하게 되면, 딸기라는 값이 name에 들어가게 된다.

```jsx
// 리덕스 스토어값 변경
export default function fruit(state = initialState, action) {
	switch(action.type) {
		case SET_FRUIT_LIST :
		    return {
		        ...state,
		        name: action.fruitList,
		    };
	    default:
		    return state;
	}
}
```

