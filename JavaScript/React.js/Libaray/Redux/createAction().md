- redux-actions 라이브러리를 통해 [[리덕스(Redux)]]의 액션([[action]])들을 관리할 수 있다.

- [[리덕스(Redux)]]에서 액션을 굳이 하나하나 [[액션 생성함수(Action Creator)]]를 만들지 않고 createAction() [[메서드(Method)]]를 통해 액션생성 자동화을 제공해준다.

- 이후 [[handleActions()]]을 사용하여 [[Reducer]]처럼 만들어준다.

## 문법

- 일단 아래는 createAction()을 쓰지 않은 일반 [[액션 생성함수(Action Creator)]]이다.

```jsx
export const increment = (index) => ({
    type: types.INCREMENT,
    index
});

export const decrement = (index) => ({
    type: types.DECREMENT,
    index
});
```

- 위의 코드를 아래와 같이 createAction()을 사용한다면 위 작업을 다음과 같이 자동화 시켜 줄 수 있다.

```jsx
export const increment = createAction(types.INCREMENT);
export const decrement = createAction(types.DECREMENT);
```

- 하지만, 이런식으로 하면 그 [[매개변수(parameter)]] 값이 index 가 될 지 뭐가 될 지 모른다.
- 그렇기 때문에, 파라미터로 전달받은 값을 액션([[action]])의 payload 값으로 설정해준다. 

## 예시

- 만약 increment(3)가 실행된다면, 다음과 같이 [[객체(Object)]]를 만들어준다.

```json
{
    type: 'INCREMENT',
    payload: 5
}
```

- 아래는 setColor의 경우이다.

```jsx
export const setColor = createAction(types.SET_COLOR);
```

```jsx
setColor({index: 5, color: '#fff'})
/* 결과:
{
    type: 'SET_COLOR',
    payload: {
        index: 5,
        color: '#fff'
    }
}
*/
```

- 액션([[action]])이 갖고있을 수 있는 [[변수(Variable)]]를 payload로 통일하므로서, 액션([[action]])을 생성하는것을 자동화 할 수 있게 되는 것이다.

- 편리하지만, 단점으로는 코드를 봤을때 해당 [[액션 생성함수(Action Creator)]]가 [[매개변수(parameter)]]로 필요한 값이 뭔지 모르기때문에, 그 위에 주석 작성해주어야 한다.

