- [[Context API]]에서 context 변화를 구독하는 [[리액트(React)]] [[컴포넌트(Component)]]이다.

- 레거시 프로젝트에서 사용되었을 수 있기 때문에 알아두면 좋다.
- 현재는 [[useContext()]]를 주로 사용한다.

- 이 컴포넌트를 사용하면 [[함수형 컴포넌트(Functional Component)]]안에서 context를 구독할 수 있다.

## 문법

```jsx
<MyContext.Consumer>
	{value => /* context 값을 이용한 렌더링 */}
</MyContext.Consumer>
```

- Context.Consumer의 자식은 [[함수(Function)]]여야한다.

- 이 함수는 context의 현재값을 받고 [[리액트(React)]] 노드를 반환한다.

- 이 [[함수(Function)]]가 받는 value [[매개변수(parameter)]] 값은 해당 context의 [[Context.Provider]] 중 상위 트리에서 가장 가까운 Provider의 [[value]] [[props]]과 동일하다.

- 상위에 [[Context.Provider]]가 없다면 value [[매개변수(parameter)]] 값은 [[createContext()]]에 보냈던 defaultValue와 동일할 것이다.