- 리덕스(Redux)란 자바스트립트 상태 관리 라이브러리이다.
- 리덕스의 본질은 [[노드(Node.js)]] [[모듈(Module)]]이다.


## Redux의 원칙

- 리덕스(Redux의)는 세 가지 원칙을 가진다.
### 1. 단일 스토어(Single source of truth)

- 동일한 데이터는 항상 같은 곳에서 가지고 온다.
- 즉, [[스토어(Store)]]라는 하나뿐인 데이터 공간이 있다는 의미이다.
### 2. 읽기 전용 상태(State is read-only)

- [[리액트(React)]]에서는 [[setState]] 혹은 [[useState()]] [[메서드(Method)]]를 활용해야만 상태([[state]]) 변경이 가능하다.
- 리덕스에서도 [[action]]이라는 [[객체(Object)]]를 통해서만 상태([[state]])를 변경할 수 있다.
- 즉, [[불변성 유지]]를 해주어야 한다.
### 3. 순수 함수만 변경 가능(Changes are made with pure functions)

- 변경은 [[순수 함수(Pure function)]]로만 가능하다.
	1. [[Reducer]]는 [[함수(Function)]]는 이전 상태([[state]])와 [[action]] [[객체(Object)]]를 [[매개변수(parameter)]]로 받는다.
	2. [[매개변수(parameter)]] 외의 값에는 의존하면 안된다.
	3. 이전 상태(prev)는 절대로 건드리지 않고, 변화를 준 새로운 상태([[state]]) [[객체(Object)]]를 만들어 반환한다.
	4. 똑같은 [[매개변수(parameter)]]로 호출된 [[Reducer]] [[함수(Function)]]는 언제나 똑같은 결과 값을 반환한다.
- 따라서 [[Reducer]] [[함수(Function)]] 내부에서 랜덤 값을 만들거나, Date 함수 사용 등을 선언하면 안된다.

- 일반 리듀서([[Reducer]])와 연관되는 개념이다.
- [[스토어(Store)]] – [[action]] – [[Reducer]] 관계를 이룬다.

![[Pasted image 20240114222341.png]]

![[Pasted image 20240114220133.png]]


## 리덕스 개념 정리
### 액션(Action)

- [[action]]은 앱에서 스토어에 운반할 데이터를 말한다.( 예) 주문서)  
- [[action]]은 자바스크립트 [[객체(Object)]] 형식으로 되어있다.

```js
{
	type: 'ACTION_CHANGE_USER', // 필수 파라미터
	payload: { // 이후 데이터(객체 포함)은 옵션
		name: '김승찬',
		age: 25
	}
}
```

### 리듀서(Reducer)

- [[action]]을 [[스토어(Store)]]에 바로 전달하는 것이 아니다.
- [[action]]을 리듀서([[Reducer]])에 전달해야한다.

- 리듀서([[Reducer]])가 주문을 보고 [[스토어(Store)]]의 상태([[state]])를 업데이트하는 것이다.
- [[action]]을 [[Reducer]]에 전달하기 위해서는 [[dispatch()]] [[메서드(Method)]]를 사용해야한다.

- [[action]] [[객체(Object)]]가 [[dispatch()]] [[메서드(Method)]]에 전달된다.
- dispatch([[action]])를 통해 [[Reducer]]를 호출한다.
- [[Reducer]]는 새로운 [[스토어(Store)]]를 생성한다.

### 스토어(Store)

- [[스토어(Store)]]는 상태가 관리되는 오직 하나의 공간이다.
- [[컴포넌트(Component)]]와는 별개로 [[스토어(Store)]]라는 공간이 있어서 그 [[스토어(Store)]] 안에 앱에서 필요한 상태([[state]])를 담는다.
- [[컴포넌트(Component)]]에서 상태([[state]]) 정보가 필요할 때 [[스토어(Store)]]에 접근한다.

## 디스패치(Dispatch)

- 디스패치는 [[스토어(Store)]]의 내장 함수 중 하나이다.
- 디스패치는 [[action]]을 발생시키는 것이다.
- [[dispatch()]]는 [[action]] [[객체(Object)]]를 [[매개변수(parameter)]]로 넣어 호출한다.
- 이 함수가 호출되면 [[스토어(Store)]]는 [[Reducer]] [[함수(Function)]]를 실행시켜서 새로운 상태([[state]])를 만들어 준다.

### 구독(subscribe)

- 구독도 [[스토어(Store)]]의 내장 함수 중 하나이다.
- subscribe [[함수(Function)]] 안에 리스너 함수를 [[매개변수(parameter)]]로 넣어서 호출해주면 이 리스너 함수가 [[action]]이 [[dispatch()]]되어 상태([[state]])가 업데이트 될 때마다 호출된다.

```jsx
const listener = () => {
	console.log('상태가 업데이트됨');
}

const unsubscibe = store.subscribe(listener);

unsubscribe(); // 추후 구독을 비활성화할 때 함수를 호출
```


## Redux의 상태 관리 도식화

- 이유는 데이터가 한 방향으로만 흘러야하기 때문이다.

[![리덕스-상태관리-도식화](https://i0.wp.com/hanamon.kr/wp-content/uploads/2021/07/%E1%84%85%E1%85%B5%E1%84%83%E1%85%A5%E1%86%A8%E1%84%89%E1%85%B3-%E1%84%89%E1%85%A1%E1%86%BC%E1%84%90%E1%85%A2%E1%84%80%E1%85%AA%E1%86%AB%E1%84%85%E1%85%B5-%E1%84%83%E1%85%A9%E1%84%89%E1%85%B5%E1%86%A8%E1%84%92%E1%85%AA.png?resize=944%2C707&ssl=1)](https://hanamon.kr/redux%eb%9e%80-%ec%83%81%ed%83%9c-%ea%b4%80%eb%a6%ac-%eb%9d%bc%ec%9d%b4%eb%b8%8c%eb%9f%ac%eb%a6%ac/%e1%84%85%e1%85%b5%e1%84%83%e1%85%a5%e1%86%a8%e1%84%89%e1%85%b3-%e1%84%89%e1%85%a1%e1%86%bc%e1%84%90%e1%85%a2%e1%84%80%e1%85%aa%e1%86%ab%e1%84%85%e1%85%b5-%e1%84%83%e1%85%a9%e1%84%89%e1%85%b5%e1%86%a8/)

## Redux에서 위 개념을 구현하는 두 가지 방법
### [[mapStateToProps]]

### Redux hooks (비교적 최근에 나온 기술)

- useSelector
- useDispatch

## Redux의 장점

- 상태([[state]])를 예측 가능하게 만든다. (순수함수를 사용하기 때문이다.)
- 유지보수에 용이하다.(복잡한 상태 관리와 비교할 수 있다.)
- 디버깅에 유리하다.(action과 state log 기록 시 redux dev tool(크롬 확장 이용한다.))
- 테스트를 붙이기 용이하다.(순수함수를 사용하기 때문이다.)

```html
<div id="red"></div> <!-- 버튼 -->

<script>
	function reducer(state, action) { // 리듀서 함수를 선언 (state, action)을 가짐
		console.log(state, action);
		if (state === undefined) {
			return { color: 'yellow' }; // 초기화를 이용한 최초의 리덕스
		}
		
		var newState; // 새로 바뀔 state를 저장할 수 있는 변수
		
		if (action.type === 'CHANGE_COLOR') { // dispatch의 type을 확인해서 같으면
			newState = Object.assign({}, state, {
				color: action.color,
			}); // 새로운 객체를 복사하여 state를 설정
		}
		
		return newState;
	}
	
	var store = Redux.createStore( // 리덕스 state를 저장하는 store 공간을 만듬
		reducer, // 첫번째 인자로 reducer를 등록
		window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__() // Redux 툴로 디버깅을 할 수 있는 코드
		);
		
		// console.log(store.getState()); 
		// store되어있는 state를 가져올려면 getState()를 호출
		
	function red() {
		var state = store.getState();
		document.querySelector('#red').innerHTML = `
			<div class="container" id = "component_red" style="background-color:${state.color}">
				<h1>red</h1>
				<input type="button" value="fire" onClick="
				store.dispatch({type:'CHANGE_COLOR', color:'red'});
				">
			</div>
		`;
	}
	
	store.subscribe(red); // red() 함수를 store에 구독해 state를 감시
	red();
```