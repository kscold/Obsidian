- [[확산 연산자(spread operator)]]가 많아짐에 따라, 프로젝트의 복잡한 상태의 코드가 만들어지는 데 이를 해소하기 위한 라이브러리이다.
- [[확산 연산자(spread operator)]]와 [[배열(Array)]] 내장 함수를 사용하여 [[불변성 유지]]하는 것은 어렵지 않지만 상태([[state]])가 복잡해진다면 조금 귀찮은 작업이 될 수도 있다.

- 따라서 immer를 사용하면 [[불변성 유지]]하는 작업을 매우 간단하게 처리할 수 있다.
- 즉 immer의 produce() 메서드를 사용하면 자동적으로 [[불변성 유지]]가 되므로 [[배열(Array)]]처럼 사용할 수 있으므로 [[push()]]나 [[splice()]], [[JavaScript/find()|find()]] [[findIndex()]]같은 [[메서드(Method)]]를 사용하기 편하다.


## produce() 문법

```jsx
import produce from 'immer';

const nextState = produce(orginalState, draft => {
	// 바꾸고 싶은 값 바꾸기
	draft.somewhere.deep.inside = 5;
})
```

- produce() [[메서드(Method)]]는 두가지 [[매개변수(parameter)]]를 받는다.

### state

- 수정하고 싶은 상태([[state]])이이다.
### draft

- 상태를 어떻게 업데이트할지 정의하는 [[함수(Function)]]이다.
- [[함수(Function)]] 내부에서 원하는 값을 변경하면, produce 함수가 [[불변성 유지]]를 대신해 주면서 새로운 상태([[state]])를 생성해준다.

- immer 라이브러리의 핵심은 불변성에 신경 쓰지 않는 것처럼 코드를 작성하되 [[불변성 유지]]는 제대로 해주는 것이다.

## 예시

- 밑의 예시는 좀 더 복잡한 데이터를 [[불변성 유지]]를 하면서 업데이트하는 예시이다.
- immer를 사용하는 경우 [[filter()]] 함수를 거의 사용하지 않기 때문에 immer을 안사용할 때가 더 간결한 경우도 많다.

```jsx
import { produce } from 'immer';

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

const nextState = produce(orginalState, draft => {
	// id가 2인 항목의 checked 값을 true로 설정
	const todo = draft.find(t => t.id === 2); // id로 항목 찾기 첫번째로 찾은 항목은 반환
	todo.checked = true; // draft[1].checked = true와 같음
	
	// 배열에 새로운 데이터 추가
	draft.push({
		id: 3,
		todo: '일정 관리 앱에 immer 적용하기',
		checked: false
	});
	
	// id = 1인 항목을 제거하기
	draft.splice(draft.findIndex(t => t.id === 1), 1) 
	// findInedx로 찾은 인덱스부터 1개 요소를 삭제, 즉 찾은 항목 삭제
});
```

## [[useState()]]의 함수형 업데이트와 immer 함께 사용

```jsx
const [number, setNumber] = useState(0);
// prevNumber는 현재 number 값을 가리킴
const onIncrease = useCallback(
	() => setNumber(prevNumber => prevNumber + 1), // 현재 값에 +1를 함
	[] // 처음 렌더링 시에만
);
```

- 밑의 예시처럼 immer에서 제동하는 produce 함수를 호출할 때, 첫 번째 [[매개변수(parameter)]]가 [[함수(Function)]] 형태라면 업데이트 함수를 반환한다.

```jsx
const update = produce(draft => {
	draft.value = 2;
});

const originalState = {
	value: 1,
	foo: 'bar',
};

const nextState = update(originalState);
console.log(nextState); // { value: 2, foo: 'bar' }
```

- 즉, 밑의 코드처럼 setter 함수 안에 produce 함수가 정의되어 있다면 첫번째 [[매개변수(parameter)]]를 생략해도 상관없다.

```jsx
const [data, setData] = useState(array: [], uselessValue: null);

const onRemove = useCallback((id) => {
	setData(
		produce((draft) => {
			draft.array.splice(
				draft.array.findIndex((info) => info.id === id), 1);
			})
		);
	}, [data]
);
```
