- [[Reducer]]에서 액션([[action]])의 type 에 따라 다른 작업을 하기 위해서 switch문을 사용했다.

- [[Reducer]]의 switch 문 대신 handleActions()를 사용하여 만들 수 있다.


## Reducer 방식의 문제 

- [[Reducer]] 방식엔 아주 중요한 결점이 한가지 있다.
- 바로, [[스코프(Scope)]]가 리듀서 함수로 설정되어있다는것이지요.

- 그렇기 때문에 서로 다른 case 에서 [[let]]이나 [[const]]를 통하여 변수를 선언하려고 하다보면 같은 이름이 중첩될시엔 에러가 발생한다.

- 그렇다고해서 각 case마다 [[함수(Function)]]를 정의하는건 코드를 읽기 힘들어질것이다.

- 따라서 handleActions를 사용하여 문제를 해결해 줄 수 있다. 

## 문법

```jsx
const reducer = handleActions({
	INCREMENT: (state, {payload: index}) => ({ // 비구조화 할당으로도 사용 가능
		counter: state.counter + index
	}),
	
	DECREMENT: (state, action) => ({
		counter: state.counter - action.payload
	})
}, { counter: 0 }); // 두번째 매개 변수는 초기값을 정의
```