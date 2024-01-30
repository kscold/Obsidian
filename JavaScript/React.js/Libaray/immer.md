- [[확산 연산자(spread operator)]]가 많아짐에 따라, 프로젝트의 복잡한 상태의 코드가 만들어지는 데 이를 해소하기 위한 라이브러리이다.


## 문법

- immer를 사용하면 [[불변성 유지]]하는 작업을 매우 간단하게 처리할 수 있다.

```jsx
import produce from 'immer';

const nextState = produce(orginalState, draft => {
	// 바꾸고 싶은 값 바꾸기
	draft.somewhere.deep.inside = 5;
})
```

```jsx
import produce from 'immer';

const originalState = [
	{
		id: 1,
		todo: '전개 연산자와 배열 내장 함수로 불변성 유지하기',
		checked: true,
	},
	{
		id: 2,
		todo: 'immer로 불변성 유지하기',
		checked: false,
	}
];

const nextState = produce(OrginalState, draft => {
	// id가 2인 항목의 checked 값을 true로 설정
	const todo = draft.find(t => t.id === 2); // id로 항목 찾기
	todo.checked = true;
	// 혹은 draft[1].checked = true;
	
	// 배열에 새로운 데이터 추가
	draft.push({
		id: 3,
		todo: '일정 관리 앱에 immer 적용하기',
		checked: false
	});
	
	// id = 1인 항목을 제거하기
	draft.splice(draft.findIndex(t => t.id === 1), 1)
});
```