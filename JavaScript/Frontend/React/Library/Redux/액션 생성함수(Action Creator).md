- 액션 생성함수는 [[action]] [[객체(Object)]]를 만들어 주는 함수이다.

- 단순히 [[매개변수(parameter)]]를 받아와서 [[action]] [[객체(Object)]] 형태로 만든다.
- 즉, [[매개변수(parameter)]]를 payload와 연결시킨다.


## 문법

```jsx
export function addTodo(data) {
	return {
	    type: "ADD_TODO",
	    payload: data
	};
}

// 화살표 함수로도 만들 수 있음
export const changeInput = text => ({ 
	type: "CHANGE_INPUT",
	payload: text
});
```

- 이러한 액션 생성함수를 만들어서 사용하는 이유는 나중에 [[컴포넌트(Component)]]에서 더욱 쉽게 [[action]]을 발생시키기 위함이다.
- 그래서 보통 [[함수(Function)]] 앞에 [[export]] [[키워드(Keyword)]]를 붙여서 다른 파일에서 불러와서 사용한다.

- [[리덕스(Redux)]]를 사용 할 때 액션 생성함수를 사용하는것이 필수적이진 않다.

- [[action]]을 발생 시킬 때마다 직접 [[action]] [[객체(Object)]]를 작성할수도 있다.