- [[함수형 컴포넌트]]에서 사용하는 [[setState]] 버전으로 [[비구조화 할당]]을 이용해서 [[const]]로 선언된 값을 업데이트한다.
- useState는 한 [[컴포넌트(component)]]에서 여러 번 사용해도 상관없다.
- 제네릭 형식으로 모든 타입이 들어 갈 수 있고, 심지어 자바스크립트는  [[일급 함수(First Class Function)]]이므로 [[객체(Object)]]도 들어갈 수 있다.



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