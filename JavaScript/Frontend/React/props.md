- [[리액트(React)]]에서 props는 properties를 줄인 표현으로 [[컴포넌트(Component)]] 속성을 선정할 때 사용하는 요소이다.

- props는 읽기 전용이므로 자식 [[컴포넌트(Component)]] 입장에서는 변하지 않는다.
- 따라서 변하게 하고자 한다면 부모 [[컴포넌트(Component)]]에서 [[state]]를 변경시켜주어야 한다.


## [[JSX]] 내부에서 props 렌더링

```jsx
import MyComponent from './MyComponent'

const App = () => {
	return <MyComponent name="React" />;
};

export default App;
```

- 속성 이름이 key값으로 실제 데이터가 value로 props가 넘어간다.

### defaultProps

- 상위 컴포넌트에서 props가 안넘어 갔을 때, 기본적으로 사용되는 것으로 [[키워드(Keyword)]]는 defaultProps이다.

```jsx
const MyComponent = props => {
	return <div>안녕하시요, 제 이름은 {props.name}입니다.</div>;
};

MyComponent.defaultProps = {
	name: '기본 이름'
}

export default MyComponent;
```

### [[children]]

- children은 리액트 컴포넌트 태그 사이의 내용을 보여주는 props이다.

```jsx
import MyCompoent from './MyComponent'

const App = () => {
	return <MyComponent>리액트</MyComponent>;
};

export default App;

// MyCompoent.js
const MyComponent = props => {
	return (
		<div>
			안녕하시요, 제 이름은 {props.name}입니다. <br />
			children 값은 {props.children}
			입니다.
		</div>
	);
};

MyComponent.defaultProps = {
	name: '기본 이름'
}

export default MyComponent;
```


## 함수 props 호출 예시

- 만약 props로 [[function]] [[키워드(Keyword)]] [[함수(Function)]]나 [[화살표 함수(Arrow function)]]를 호출한다면 ()=>함수() 를 통해서 렌더링되자마자 호출되는 것을 막을 수 있다.

```jsx
function App() {
	const handleSelect = (selectedButton) => {
		console.log('Hello World - selected!');
	};
	
	return <TapButton onSelect={() => handleSelect()}>Components</TapButton>
	// 만약 handleSelct()로 호출했다면 렌더링과 동시에 호출이 되어버림
}	
```

- 또한 위의 코드처럼 사용하면 자식 [[컴포넌트(Component)]]로 onSelect로 [[함수(Function)]]를 props로 보낸때 [[매개변수(parameter)]]를 넣어 [[함수(Function)]] 호출할 수 있고 자식 [[컴포넌트(Component)]]에서 원하는 때에 이 [[함수(Function)]]를 호출할 수 있게 된다.

```jsx
// App.js
function App() {
	const handleSelect = (selectedButton) => {
		console.log('Hello World - selected!');
	};
	
	return <TapButton onSelect={() => handleSelect('components')}>Components</TapButton>
	// 만약 handleSelct()로 호출했다면 렌더링과 동시에 호출이 되어버림
}	


// TapButton.js
const TapButton = ({ children, onSelect }) => { 
	
	return ( // 자식 컴포넌트의 버튼이 눌렸을 때 부모의 handleSelect 함수를 호출
		<li>
			<button onClick={onSelect}>{children}</button> 
		</li>
	);
};

  
export default TapButton;
```

## [[비구조화 할당]]을 사용한 예시

```jsx
// MyCompoent.js
const MyComponent = props => {
	const { name, children } = props;
	
	return (
		<div>
			안녕하시요, 제 이름은 {name}입니다. <br />
			children 값은 {children}
			입니다.
		</div>
	);
};

MyComponent.defaultProps = {
	name: '기본 이름'
}

export default MyComponent;
```

### propTypes를 통한 props 검증

```jsx
// MyCompoent.js
import PropTypes from 'prop-types';

const MyComponent = ({ name, children }) => {
	return (...)
};

MyComponent.defaultProps = {
	name: '기본 이름'
};

MyComponent.propTypes = {
	name: PropTypes.string
};

export default MyComponent;

// App.js
import MyComponent from './MyComponent';

const App = () => {
	return <MyComponent name={3}>리액트</MyComponent>; // name 3이기 때문에 propTypes에 의해 오류가 남
}

export default App;
```

- 만약 컴포넌트에 선정한 props가 propTypes에서 지정한 형태와 일치하지 않는다면 브라우저 개발자 도구의 Console 탭에 propTypes가 잘못되었다는 것을 알려준다. (오류 메시지 확인용)

### isRequired를 사용하여 필수 propTypes 설정
- propTypes를 지정할 때 isRequired를 붙여서 필수 props를 지정하지 않았을 때 경고 메세지를 띄워준다.

```jsx
// MyCompoent.js
import PropTypes from 'prop-types';

const MyComponent = ({ name, favoriteNumber, children }) => {
	return (
		<div>
			안녕하시요, 제 이름은 {name}입니다. <br />
			children 값은 {children}
			입니다.
			<br />
			제가 좋아하는 숫자는 {favoriteNumber}입니다.
		</div>
	);
};

MyComponent.defaultProps = {
	name: '기본 이름'
};

MyComponent.propTypes = {
	name: PropTypes.string,
	favoriteNumber: PropTypes.number.isRequired
};

export default MyComponent;

