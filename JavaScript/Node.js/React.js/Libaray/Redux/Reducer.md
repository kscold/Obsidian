- [[action]]을 [[스토어(Store)]]에 바로 전달하는 것이 아니고 리듀서([[Reducer]])에 전달해야한다.

- 따라서 리듀서([[Reducer]])가 주문을 보고 [[스토어(Store)]]의 상태([[state]])를 업데이트하는 것이다.

- [[action]]을 [[Reducer]]에 전달하기 위해서는 [[dispatch()]] [[메서드(Method)]]를 사용해야한다.
- [[action]] [[객체(Object)]]가 [[dispatch()]] [[메서드(Method)]]에 전달된다.
- dispatch([[action]])를 통해 [[Reducer]]를 호출한다.

- [[Reducer]]는 새로운 [[스토어(Store)]]를 생성한다.

- 즉, 리듀서는 변화를 일으키는 [[함수(Function)]]로 현재의 상태([[state]])와 [[action]]을 참조하여 새로운 상태([[state]])를 반환한다.

## 문법

- 다음과 같이 reducer [[함수(Function)]]를 선언할 떄는 두가지의 [[매개변수(parameter)]]를 만든다.

```jsx
function reducer(state, action) {
  // 상태 업데이트 로직
  return alteredState;
}
```

- 리듀서는, 현재의 상태([[state]])와, 전달 받은 [[action]]을 참고하여 새로운 상태([[state]])를 만들어서 반환한다.
- 첫번째 상태는 가장 최신의 snopshot으로 보장된 상태([[state]])이다. 
- (prev) => 라고 생각하면 편하다.

- 이 리듀서는 [[useReducer()]] 를 사용할때 작성하는 리듀서와 똑같은 형태를 가지고 있다.


## 예시

- 만약 카운터를 위한 리듀서를 작성한다면 밑의 코드와 같이 작성할 수 있다.

```jsx
function counter(state, action) {
	switch (action.type) {
	    case 'INCREASE':
			return state + 1;
		case 'DECREASE':
		    return state - 1;
	    default:
		    return state;
	}
}
```

- [[useReducer()]] 에선 일반적으로 default: 부분에 throw new Error('Unhandled Action')과 같이 에러를 발생시키도록 처리하는게 일반적이다.
- 그러나 [[리덕스(Redux)]]의 리듀서에서는 기존 [[state]]를 그대로 반환하도록 작성해야 한다.

- [[리덕스(Redux)]]를 사용 할 때에는 여러 개의 리듀서를 만들고 이를 합쳐서 루트 리듀서(Root Reducer)를 만들 수 있다.
- 루트 리듀서(Root Reducer) 안의 작은 리듀서들은 서브 리듀서(Sub Reducer)라고 부른다.