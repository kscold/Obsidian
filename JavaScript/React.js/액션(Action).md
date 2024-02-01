- 상태([[state]])에 어떠한 변화가 필요하게 될 땐, 우리는 액션이란 것을 발생시킨다.
- 이는, 하나의 [[객체(Object)]]로 표현된다.

```jsx
{
  type: "TOGGLE_VALUE"
}
```

- action [[객체(Object)]]는 type [[속성(Property)]]를 필수적으로 가지고 있어야하고 그 외의 값들은 개발자 마음대로 넣어줄 수 있다.

```jsx
{
  type: "ADD_TODO", // action 객체의 type 속성은 필수적으로 가지고 있어야 함
  data: {
    id: 0,
    text: "리덕스 배우기"
  }
}
```

```jsx
{
  type: "CHANGE_INPUT",
  text: "안녕하세요"
}
```

### 액션 생성함수 (Action Creator)

- action을 만드는 함수로 사용할 수 있다.
- 단순히 매개변수를 받아와서 액션 객체 형태로 만든다.

```jsx
export function addTodo(data) {
  return {
    type: "ADD_TODO",
    data
  };
}

// 화살표 함수로도 만들 수 있음
export const changeInput = text => ({ 
  type: "CHANGE_INPUT",
  text
});
```

- 이러한 액션 생성함수를 만들어서 사용하는 이유는 나중에 [[컴포넌트(component)]]에서 더욱 쉽게 액션을 발생시키기 위함이다.
- 그래서 보통 함수 앞에 export [[키워드(Keyword)]]를 붙여서 다른 파일에서 불러와서 사용한다.

- [[리덕스(Redux)]]를 사용 할 때 acition 생성함수를 사용하는것이 필수적이진 않다.
- action을 발생 시킬 때마다 직접 aciton 객체를 작성할수도 있다.