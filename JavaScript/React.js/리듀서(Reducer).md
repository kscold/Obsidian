- 변화를 일으키는 함수로 현재의 상태([[state]])와 액션을 참조하여 새로운 상태를 반환한다.

- reducer(리듀서)는 두가지의 매개변수를 받아온다.

```jsx
function reducer(state, action) {
  // 상태 업데이트 로직
  return alteredState;
}
```

- 리듀서는, 현재의 상태([[state]])와, 전달 받은 [[액션(Action)]]을 참고하여 새로운 상태([[state]])를 만들어서 반환한다.
- 이 리듀서는 [[useReducer()]] 를 사용할때 작성하는 리듀서와 똑같은 형태를 가지고 있다.

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

- [[useReducer()]] 에선 일반적으로 `default:` 부분에 `throw new Error('Unhandled Action')`과 같이 에러를 발생시키도록 처리하는게 일반적이다.
- 그러나 [[리덕스(Redux)]]의 리듀서에서는 기존 [[state]]를 그대로 반환하도록 작성해야한다.

- [[리덕스(Redux)]]를 사용 할 때에는 여러개의 리듀서를 만들고 이를 합쳐서 루트 리듀서 (Root Reducer)를 만들 수 있다.(루트 리듀서 안의 작은 리듀서들은 서브 리듀서라고 부른다.)