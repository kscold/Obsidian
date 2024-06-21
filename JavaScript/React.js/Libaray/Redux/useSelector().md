- useSelector()는 [[리액트(React)]]의 [[리덕스(Redux)]] [[스토어(Store)]] 관련 [[Hooks]]중 하나이다.
- 이 [[Hooks]]은 [[스토어(Store)]]의 상태([[state]])값을 반환해주는 역할을 한다.

- useSelector를 사용한 [[함수(Function)]]에서 [[리덕스(Redux)]] [[스토어(Store)]]의 상태값이 바뀐 경우( 버튼 클릭 등의 이벤트를 통해 액션이 실행되어 상태값이 변경된 경우) 바뀐 [[스토어(Store)]]의 상태([[state]])값을 다시 가져와 [[컴포넌트(Component)]]를 렌더링시킨다.

## 문법

```jsx
import { useSelector } from 'react-redux';

const fruit = useSelector(state => state.fruit);
```

- useSelector를 사용하여 상태값을 가져오는 것은 간단하다.
- useSelector에 매개변수에 state => state.모듈명  형식으로 상태값을 반환할 수 있다. 

```jsx
import { useSelector } from 'react-redux';

const name = useSelector(state => state.fruit.name);
const price = useSelector(state => state.fruit.price);

// 비구조화 할당 문법을 사용
const {name, price} = useSelector(({fruit}) => ({
	name: fruit.name,
	price: fruit.price
}));
```

- 위의 예시처럼 들어 fruit 상태([[state]]) 안에 name, price 등의 요소를 바로 가져오고 싶을 경우, state => state.fruit.name   state => state.fruit.price 와 같이 요소요소들을 마침표로 구분하여 상태값을 가져올 수도 있고 [[비구조화 할당]] 문법을 사용하여 바로 [[속성(Property)]]을 추출해도 상관없다.


## 예시

- 아래 코드는 [[Reducer]] [[함수(Function)]]를 사용한 예시이다.

```jsx
// modules/fruit.js

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

- 아래 코드는 [[createAction()]]과 [[handleActions()]]를 사용한 예시이다.

```jsx
// modules/fruit.js

// 액션
const SET_FRUIT_LIST = "fruit/SET_FRUIT_LIST";

// 액션 생성 함수
export const setFruitList = createAction(SET_FRUIT_LIST);

// 초기값
const initialState = {
	name: false,
	price: false,
};

const fruit = handleActions(
	[SET_FRUIT_LIST]: (state, { payload: fruitList }) => ({
		...state
		name: fruitList
	})
)
```

- 이전 게시글의 액션을 바탕으로, fruitList에 '딸기'라는 값이 들어간 상태로 setFruitList라는 액션이 실행되었다고 가정해보자.
- setFruitList 액션으로 인해, fruit이라는 모듈의 name 상태에 '딸기'라는 값이 들어갔다.