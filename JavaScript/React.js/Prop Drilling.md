- prop Drilling은 [[props]]를 오로지 하위 [[컴포넌트(Component)]]로 전달하는 용도로만 쓰이는 [[컴포넌트(Component)]]들을 거치면서 React Component 트리의 한 부분에서 다른 부분으로 데이터를 전달하는 과정이다.

## 예시

```jsx
import React from "react";
import "./styles.css";

export default function App() {
	return (
    <div className="App">
      <FirstComponent content="Who needs me?" />
    </div>
  );
}

function FirstComponent({ content }) {
  return (
    <div>
      <h3>I am the first component</h3>;
      <SecondComponent content={content} />|
    </div>
  );
}

function SecondComponent({ content }) {
  return (
    <div>
      <h3>I am the second component</h3>;
      <ThirdComponent content={content} />
    </div>
  );
}

function ThirdComponent({ content }) {
  return (
    <div>
      <h3>I am the third component</h3>;
      <ComponentNeedingProps content={content} />
    </div>
  );
}

function ComponentNeedingProps({ content }) {
  return <h3>{content}</h3>;
}
```

- 위의 예시를 보면 content를 App > First > Second > Third > ComponentNeedingProps순으로 전달한다.
- ComponentNeedingProps 컴포넌트에서 해당 [[props]]를 사용하기 위해 전달하는 과정을 거치게 된다.

### Prop Drilling의 문제

- 우선 `Prop Drilling` 는 문제가 되지 않는다.
- [[props]] 전달이 3~5개 정도의 컴포넌트라면 말이다.

- 하지만 prop 전달이 10개, 15개 같이 더 많은 과정을 거치게 된다면 코드를 읽을 때 해당 prop을 추적하기 힘들어진다.
- 그렇기 때문에 유지보수도 더욱 어려워진다.

## Prop drilling 해결하는 방법

1. [[Context API]]
     - React의 Context API를 사용하여 데이터를 전역적으로 공유할 수 있다.
     - Context를 생성하고 값을 제공하는 컴포넌트를 작성한 다음, 필요한 컴포넌트에서 useContext 훅을 사용하여 해당 값을 직접 접근할 수 있다.
     - 이를 통해 중간 컴포넌트를 거치지 않고도 데이터를 전달할 수 있다.

1. [[리덕스(Redux)]] 또는 다른 상태 관리 라이브러리  
	- 첫 번째로 전역 상태관리 라이브러리 사용 [[리덕스(Redux)]], MobX, recoil 등을 사용하여 해당 값이 필요한 컴포넌트에서 직접 불러서 사용할 수 있다.
	- Redux와 같은 상태 관리 라이브러리를 사용하면 애플리케이션의 상태를 중앙에서 관리할 수 있다.
	- 상태를 저장하고 필요한 컴포넌트에서 상태를 가져와 사용할 수 있다.
	- 이를 통해 prop drilling을 피하고 상태를 전역적으로 공유할 수 있다.

3. [[Custom Hooks]]
	- Custom Hooks를 사용하여 관련된 로직을 재사용 가능한 함수로 추상화할 수 있다.
	- 커스텀 훅 내에서 상태와 로직을 처리하고, 필요한 컴포넌트에서 해당 훅을 호출하여 데이터를 가져올 수 있다.
	- 이를 통해 prop drilling을 해소하고 데이터 전달을 보다 간편하게 할 수 있다.

4. Render Props 패턴과 Children props:  
    - Render Props 패턴이나 Children props를 활용하여 데이터를 부모 컴포넌트에서 자식 컴포넌트로 전달할 수 있다.  
    - Render Props 패턴은 부모 컴포넌트에서 함수를 정의하고, 자식 컴포넌트에서 해당 함수를 호출하여 데이터를 전달받을 수 있다.
    - Children props는 부모 컴포넌트에서 컴포넌트 태그 사이의 내용을 자식 컴포넌트로 전달한다.
### Children props의 예시 

```jsx
import React from "react";
import "./styles.css";

export default function App() {
	const content = "Who needs me?";
	
	return (
		<div className="App">
			<FirstComponent>
		      <SecondComponent>
		          <ThirdComponent>
			          <ComponentNeedingProps content={content} />
		          </ThirdComponent>
	          </SecondComponent>
		    </FirstComponent>
		</div>
	);
}

function FirstComponent({ children }) {
	return (
		<div>
		    <h3>I am the first component</h3>;
			{children}
		</div>
	);
}

function SecondComponent({ children }) {
	return (
		<div>
		    <h3>I am the second component</h3>;
		    {children}
	    </div>
	);
}

function ThirdComponent({ children }) {
	return (
	    <div>
		    <h3>I am the third component</h3>
			{children}
		</div>
	);
}

function ComponentNeedingProps({ content }) {
	return <h3>{content}</h3>
}
```

- 이렇게 리팩토링을 진행한다면 하나의 컴포넌트에서 값을 관리하고, 그 값을 하위요소로 전달할 때 전혀 코드의 추적이 어려워지지 않는다.