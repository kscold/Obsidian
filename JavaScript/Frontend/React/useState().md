- [[리액트(React)]]의 경우 [[SPA(Single Page Application)]]로 이루어진 [[Virtual DOM]]의 트리 구조를 가지고 있으므로 [[컴포넌트(Component)]] 간의 [[props]]가 이동될 때 일반적인 [[let]]이나 [[const]] [[키워드(Keyword)]]를 통해 [[변수(Variable)]]나 상수를 선언하면 변화를 감지하지 못해 [[리렌더링(Re-rendering)]]이 일어나지 않는다.

- [[함수형 컴포넌트(Functional Component)]]에서 사용하는 [[setState]] 버전으로 [[비구조화 할당]]을 이용해서 [[const]]로 선언된 값을 업데이트한다.
- useState는 한 [[컴포넌트(Component)]]에서 여러 번 사용해도 상관없다.

- [[제네릭(Generic)]] 형식으로 모든 타입이 들어 갈 수 있고, 심지어 자바스크립트는  [[일급 함수(First Class Function)]]이므로 [[객체(Object)]]도 들어갈 수 있다.


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

## 함수형 업데이트(Functional Update)

- 이전 상태를 기반으로 새 상태를 계산해야 할 때 `setState(prev => newValue)` 형태를 사용한다.
- [[비동기(asynchronous)]] 콜백이나 [[클로저(Closure)]] 안에서 최신 상태를 안전하게 참조할 수 있다.

```js
// 잘못된 방법 — 동시 호출 시 최신 값 반영 안 됨
setCount(count + 1);
setCount(count + 1); // 두 번 호출해도 1만 증가

// 올바른 방법 — 항상 이전 상태 기반으로 증가
setCount(prev => prev + 1);
setCount(prev => prev + 1); // 정확히 2 증가
```

## 스테일 클로저(Stale Closure) 문제

- `setTimeout`, 이벤트 핸들러 등 [[클로저(Closure)]] 안에서 `state`를 참조하면, 클로저가 생성된 시점의 값을 캡처한다.
- 이 문제는 [[useRef()]]로 최신 상태를 추적하거나, 함수형 업데이트로 해결한다.

```js
function Counter() {
  const [count, setCount] = useState(0);

  const handleDelayedAlert = () => {
    setTimeout(() => {
      // 클릭 시점의 count를 캡처 — 이후 setCount 호출해도 이 값은 바뀌지 않음
      alert(`클릭 시점의 count: ${count}`);
    }, 3000);
  };

  return (
    <>
      <p>{count}</p>
      <button onClick={() => setCount(c => c + 1)}>증가</button>
      <button onClick={handleDelayedAlert}>3초 후 알림</button>
    </>
  );
}
```

## 객체/배열 상태 업데이트

- `state`가 [[객체(Object)]]나 배열인 경우, 반드시 **새 참조**를 만들어야 React가 변경을 감지한다.

```js
// 잘못된 방법 — 기존 객체 직접 수정
const [user, setUser] = useState({ name: '홍길동', age: 20 });
user.age = 21; // 참조 동일 → 리렌더링 안 됨
setUser(user);

// 올바른 방법 — 스프레드로 새 객체 생성
setUser(prev => ({ ...prev, age: 21 }));

// 배열 — push 금지, 새 배열 생성
setItems(prev => [...prev, newItem]);
setItems(prev => prev.filter(item => item.id !== targetId));
```
