- useCallback는 [[함수(Function)]]를 메모이제이션하고 [[함수(Function)]]를 반환한다.

- 원래 [[컴포넌트(Component)]]가 렌더링 될 때 그 안에 있는 [[함수(Function)]]도 다시 만들게 된다.
- 하지만 똑같은 [[함수(Function)]]를 [[컴포넌트(Component)]]가 렌더링 된다고 해서 계속 다시 만드는 것은 좋은 현상이 아니다.
- 만약, 이 함수가 자식 [[컴포넌트(Component)]]로 [[props]]를 내려준다면 [[함수(Function)]]를 포함하고 있는 [[컴포넌트(Component)]]가 [[리렌더링(Re-rendering)]] 될 때마다 자식 [[컴포넌트(Component)]]도 [[함수(Function)]]가 새롭게 만들어지니 계속 [[리렌더링(Re-rendering)]]하게 된다.

- 따라서 useCallback은 자주 렌더링되는 [[컴포넌트(Component)]]에서 함수를 최적화하고, 불필요한 함수 재생성을 방지하는 데 사용한다.

- 예를 들어, 자식 컴포넌트에 전달되는 [[콜백 함수(Callback Function)]]를 최적화하고 싶은 경우나, 외부에서 값을 가져오는 api를 호출하는 경우에 사용한다.


## 이벤트 핸들러 함수가 자주 재생성되는 경우

- useCallback을 사용하지 않으면 이벤트 핸들러 함수는 매번 새로운 [[인스턴스(Instance)]]가 생성된다.
- 그러나 useCallback을 사용하면 함수가 처음 생성될 때 한 번만 생성되며, 나중에는 동일한 함수 인스턴스를 재사용하게 된다.


## 자식 컴포넌트에 props로 함수가 자주 재생성되는 경우

- 먼저 [[함수(Function)]]는 값이 아닌 참조([[참조 타입(Reference Type)]])로 비교된다.

```js
const functionOne = function() {
	return 5;
};

const functionTwo = function() {
	return 5;
};

// 서로의 참조가 다르기 때문에 false기 나옴
console.log(functionOne === functionTwo);
```

- 동일한 값을 반환하지만 참조가 다르기 때문에 false가 나온다.
- 위와 같이 [[컴포넌트(Component)]]에서 특정 함수를 정의할 경우 각각의 함수들은 모두 고유한 함수가 된다.

- 이런 고유한 함수([[순수 함수(Pure function)]])가 생성될 경우, 부모를 통해 [[props]]에 함수를 전달받는 자식 [[컴포넌트(Component)]]에서는 [[props]]가 변경되었다고 판단해 [[리렌더링(Re-rendering)]]이 발생하게 된다.

```jsx
function App() {
	const [name, setName] = useState('');
	const onSave = () => {};
	
	return (
		<div className="App">
		    <input
			    type="text"
			    value={name}
			    onChange={(e) => setName(e.target.value)}
		    />
		    <Profile onSave={onSave} />
		</div>
	);
}
```

- useCallback을 사용하지 않을 경우, name이 변경되어 [[리렌더링(Re-rendering)]]이 발생하면 onSave함수가 새로 만들어지고, Profile 컴포넌트의 [[props]]로 onSave함수가 새로 전달되게 된다.

- 이때 Profile 컴포넌트에서 [[useMemo()]]를 사용해도 이전 onSave와 이후 onSave가 같은 값을 반환하지만 참조(주소)가 다른 [[함수(Function)]]([[객체(Object)]])가 되어버리기 때문에 리렌더링이 일어나게 된다.
 - 부모 컴포넌트만 수정하려고 했지만 연쇄적으로 하위 컴포넌트들 모두 렌더링이 일어나게 된다.

- 따라서 아래와 같이 useCallback을 사용해서 onSave라는 함수를 재사용하는 것으로 자식 컴포넌트의 리렌더링을 방지할 수 있다.

```jsx
import React, { useCallback, useState } from 'react';
import Profile from './Profile';


function App() {
  const [name, setName] = useState('');
  const onSave = useCallback(() => {
    console.log(name);
  }, [name]);

  return (
    <div className="App">
      <input
        type="text"
        value={name}
        onChange={(e) => setName(e.target.value)}
      />
      <Profile onSave={onSave} />
    </div>
  );
}
```

- useCallback을 사용해서 onSave라는 함수를 재사용하는 것으로 자식 컴포넌트의 리렌더링을 방지할 수 있다.

- 또 다른 [[콜백 함수(Callback Function)]]가 자식 컴포넌트에 props로 전달되는 예시이다.

```jsx
// ParentComponent.jsx
function ParentComponent() {
	const [count, setCount] = useState(0);
	
	const handleClick = useCallback(() => {
		setCount(prevCount => prevCount + 1);
	}, []);
	
	return (
		<div>
			<ChildComponent onClick={handleClick} />
		</div>
	);
}

// ChildComponent.jsx
function ChildComponent({ onClick }) {

	return (
		<button onClick={onClick}>Click me</button>
	);
}
```

- handleClick 함수가 ParentComponent 컴포넌트에서 생성되어 ChildComponent 컴포넌트에 props로 전달된다. 
- 만약 useCallback을 사용하지 않으면 ParentComponent가 리렌더링 될 때마다 handleClick 함수가 새로 생성된다. 
- 이 경우 ChildComponent는 onClick props가 변경되었으므로 불필요한 리렌더링이 발생하게 된다. 이를 방지하기 위해 useCallback을 사용하여 handleClick 함수를 메모이제이션한다.

## 렌더링 최적화가 필요한 경우

- useCallback을 사용하여 함수를 재사용하면, 렌더링 최적화를 수행할 수 있다. 
- 이는 컴포넌트의 불필요한 재렌더링을 방지하고, 성능을 향상시킬 수 있다.
## 외부에서 값을 가져오는 api를 호출하는 경우

```jsx
import React, { useState, useEffect } from "react";

function Profile({ userId }) {
	const [user, setUser] = useState(null);
	
	const fetchUser = () =>
		fetch(`https://your-api.com/users/${userId}`)
		.then((response) => response.json())
		.then(({ user }) => user);
	
	useEffect(() => {
		fetchUser().then((user) => setUser(user));
	}, [fetchUser]);
	
	// ...
}
```

- 위의 코드는 fetchUser 함수가 변경될 때만 외부에서 api를 가져와 [[useEffect()]]가 실행된다.

- 사실 이 코드는 정상적인 코드가 아니다.
- Profile이라는 컴포넌트가 리렌더링이 발생할 경우 fetchUser 함수에는 새로운 함수가 할당되게 된다.
- 그러면 [[useEffect()]]함수가 호출되어 user 상태값이 바뀌고, [[state]] 값이 바뀌었기 때문에 다시 리렌더링이 일어난다.
- 즉, 무한루프에 빠져버리게 된다.

- 이때 useCallback을 사용할 경우 fetchUser 함수의 참조값을 동일하게 유지시킬 수 있다.

```jsx
import React, { useState, useEffect } from "react";

function Profile({ userId }) {
	const [user, setUser] = useState(null);
	
	const fetchUser = useCallback(() =>
		fetch(`https://your-api.com/users/${userId}`)
		.then((response) => response.json())
        .then(({ user }) => user),
      [userId]);
      
	useEffect(() => {
	    fetchUser().then((user) => setUser(user));
	}, [fetchUser]);
	
	// ...
}
```

- api의 옵션으로 사용되는 userId가 변동될 때만 fetchUser에 새로운 함수가 할당되도록 설정하고, 그것이 아니면 동일한 함수가 실행되게 되서 무한 루프에 빠지지 않도록 할 수 있다.