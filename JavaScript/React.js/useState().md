- [[리액트(React)]]의 경우 [[SPA(Single Page Application)]]로 이루어진 [[Virtual DOM]]의 트리 구조를 가지고 있으므로 [[컴포넌트(Component)]] 간의 [[props]]가 이동될 때 일반적인 [[let]]이나 [[const]] [[키워드(Keyword)]]를 통해 [[변수(Variable)]]나 상수를 선언하면 변화를 감지하지 못해 [[리렌더링(Re-rendering)]]이 일어나지 않는다.

- [[함수형 컴포넌트(Functional Component)]]에서 사용하는 [[setState]] 버전으로 [[비구조화 할당]]을 이용해서 [[const]]로 선언된 값을 업데이트한다.
- useState는 한 [[컴포넌트(Component)]]에서 여러 번 사용해도 상관없다.

- 제네릭 형식으로 모든 타입이 들어 갈 수 있고, 심지어 자바스크립트는  [[일급 함수(First Class Function)]]이므로 [[객체(Object)]]도 들어갈 수 있다.


- 따라서 [[리액트(React)]]는 useState를 사용해서 상수형인 [[const]]를 [[변수(Variable)]]처럼 사용하고 [[불변성 유지]]를 하며 업데이트 한다.


## useState의 함수형 업데이트

- setter함수를 통해 새로운 상태를 매개변수로 넣는 대신 상태 업데이트를 어떻게 할지 결정해 주는 함수를 넣을 수도 있고, 이를 함수형 업데이트라고 부른다.

```jsx
const [number, setNumber] = useState(0);

const onIncrease = useCallback(
	() => setNumber(prevNumber => prevNumber + 1), []);
// 매개변수로 prevNumber를 받아서 상태를 결정함
```


-  userState는 아래 예시처럼 [[객체(Object)]]도 받을 수 있다.

```jsx
const [form, setForm] = useState({ username: '', message: '' });
```