- Redux(리덕스)란 자바스트립트 상태관리 라이브러리이다.
- Redux(리덕스)의 본질은 Node.js 모듈이다.

## Redux의 기본 개념 

- 세 가지 원칙을 가진다.
### 1. Single source of truth

- 동일한 데이터는 항상 같은 곳에서 가지고 온다.
- 즉, 스토어라는 하나뿐인 데이터 공간이 있다는 의미이다.
### 2. State is read-only

- 리액트에서는 [[setState]] 혹은 [[useState()]] [[메서드(Method)]]를 활용해야만 상태([[state]]) 변경이 가능하다.
- 리덕스에서도 action이라는 [[객체(Object)]]를 통해서만 상태([[state]])를 변경할 수 있다.
### 3. Changes are made with pure functions

- 변경은 순수함수로만 가능하다.
- 리듀서와 연관되는 개념이다.
- [[스토어(Store)]] – [[액션(Action)]] – [[리듀서(Reducer)]] 관계를 이룬다.

![[Pasted image 20240114222341.png]]

![[Pasted image 20240114220133.png]]

## Store(스토어)

- [[스토어(Store)]]는 상태가 관리되는 오직 하나의 공간이다.
- [[컴포넌트(component)]]와는 별개로 스토어라는 공간이 있어서 그 스토어 안에 앱에서 필요한 상태([[state]])를 담는다.
- 컴포넌트에서 상태 정보가 필요할 때 스토어에 접근한다.

## Action(액션)

- action(액션)은 앱에서 스토어에 운반할 데이터를 말한다.(예) 주문서)  
- [[액션(Action)]](액션)은 자바스크립트 [[객체(Object)]] 형식으로 되어있다.

```js
{
	type: 'ACTION_CHANGE_USER', // 필수 파라미터
	payload: { // 이후 데이터(객체 포함)은 옵션
		name: '김승찬',
		age: 25
	}
}
```

## Reducer(리듀서)

- Action(액션)을 Store(스토어)에 바로 전달하는 것이 아니다.
- Action(액션)을 [[리듀서(Reducer)]](리듀서)에 전달해야한다.
- Reducer(리듀서)가 주문을 보고 Store(스토어)의 상태를 업데이트하는 것이다.
- Action(액션)을 Reducer(리듀서)에 전달하기 위해서는 dispatch() [[메서드(Method)]]를 사용해야한다.

- `Action(액션) 객체`가 [[dispatch()]] 메서드에 전달된다.
- `dispatch(액션)`를 통해 `Reducer`를 호출한다.
- `Reducer`는 새로운 `Store` 를 생성한다.

## Redux의 상태 관리 도식화

- 이유는 데이터가 한 방향으로만 흘러야하기 때문이다.

[![리덕스-상태관리-도식화](https://i0.wp.com/hanamon.kr/wp-content/uploads/2021/07/%E1%84%85%E1%85%B5%E1%84%83%E1%85%A5%E1%86%A8%E1%84%89%E1%85%B3-%E1%84%89%E1%85%A1%E1%86%BC%E1%84%90%E1%85%A2%E1%84%80%E1%85%AA%E1%86%AB%E1%84%85%E1%85%B5-%E1%84%83%E1%85%A9%E1%84%89%E1%85%B5%E1%86%A8%E1%84%92%E1%85%AA.png?resize=944%2C707&ssl=1)](https://hanamon.kr/redux%eb%9e%80-%ec%83%81%ed%83%9c-%ea%b4%80%eb%a6%ac-%eb%9d%bc%ec%9d%b4%eb%b8%8c%eb%9f%ac%eb%a6%ac/%e1%84%85%e1%85%b5%e1%84%83%e1%85%a5%e1%86%a8%e1%84%89%e1%85%b3-%e1%84%89%e1%85%a1%e1%86%bc%e1%84%90%e1%85%a2%e1%84%80%e1%85%aa%e1%86%ab%e1%84%85%e1%85%b5-%e1%84%83%e1%85%a9%e1%84%89%e1%85%b5%e1%86%a8/)

## Redux에서 위 개념을 구현하는 두 가지 방법
### mapStateToProps()

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