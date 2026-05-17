- 상태([[state]])에 어떠한 변화가 필요하게 될 땐, [[Reducer]]는 액션이란 것을 발생시킨다.

- 이는, 하나의 [[객체(Object)]]로 표현된다.

- 어떤 변화를 일으켜야 할 때마다 action [[객체(Object)]]를 만들어야 하기 때문에 [[액션 생성함수(Action Creator)]]를 사용한다.


## 문법

```jsx
{
	type: "TOGGLE_VALUE"
}
```

- action [[객체(Object)]]는 type [[속성(Property)]]를 필수적으로 가지고 있어야하고 그 외의 값들은 개발자 마음대로 넣어줄 수 있다.

```jsx
{
	type: "ADD_TODO", // action 객체의 type 속성은 필수적으로 가지고 있어야 함
	payload: { // 보통 데이터의 이름은 payload 또는 data로 선언
		id: 0,
		text: "리덕스 배우기"
	}
}
```

```jsx
{
	type: "CHANGE_INPUT",
	text: "안녕하세요" // 개발자가 마음대로 이름을 정의
}
```