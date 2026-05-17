- 디스패치(dispatch)는 [[스토어(Store)]]의 내장 함수 중 하나이다.

- [[리덕스(Redux)]]나 [[useReducer()]]에서 [[action]]을 발생시키는 [[함수(Function)]]이다.

- dispatch([[action]])과 같은 형태로 [[함수(Function)]] 안에 [[매개변수(parameter)]]로로 [[action]] 값을 넣어 주면 [[Reducer]] 함수가 호출되는 구조이다.

- 호출을 하면, [[스토어(Store)]]는 리듀서 함수([[Reducer]])를 실행시켜서 해당 액션을 처리하는 로직이 있다면 액션([[action]])을 참고하여 새로운 상태([[state]])를 만들어준다.


## 예시

- 밑의 코드와 같이 [[Reducer]] [[함수(Function)]]와 시킨 [[Custom Hooks]]를 만들어, dispatch를 통해 [[이벤트(event)]]를 갱신하여 부모 [[컴포넌트(Component)]]로 부터 받은 [[state]]를 반환할 수 있다.

```jsx
// 리듀서 커스텀 훅 선언
import { useReducer } from 'react';

function reducer(state, action) { // 리듀서 함수 선언
	return {
		...state, // 이전의 state를 가져옴(일전의 snapshot를 잃지 않기 위해 쓰는 것이 좋음)
		// action에 payload 즉, e.target이 할당됨 
		[action.name]: action.value, // 유동적으로 새로운 state로 만듬
	};
}

export default function useInput(initialForm) { 
	// 부모 컴포넌트로 부터 매개변수로 initialForm(초기값) 객체를 받음
	const [state, dispatch] = useReducer(reducer, initialForm); // 리듀서 함수와 초기값
	const onChange = (e) => {
		dispatch(e.target); // dispatch를 통해 e.target를 갱신
	};
	
	return [state, onChange];
}
```

- 아래는 [[Custom Hooks]]을 사용하는 부분이다.

```jsx
import useInput from './useInput';

const Info = () => {
	const [state, onChange] = useInput({
		name: '',
		nickname: '',
	});
	
	const { name, nickname } = state; // 바뀐 onChange state를 비구조화 할당
	
	return (
		<div>
			<div>
				<input name="name" value={name} onChange={onChange} />
				<input name="nickname" value={nickname} onChange={onChange} />
			</div>
			<div>
				<div>
					이름:
					<b /> {name}
				</div>
				<div>
					닉네임:
					<b /> {nickname}
				</div>
			</div>
		</div>
	);
};

export default Info;
```