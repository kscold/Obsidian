- props는 properties를 줄인 표현으로 컴포넌트 속성을 선정할 때 사용하는 요소이다.

### JSX 내부에서 props 렌더링

```jsx
import MyComponent from './MyComponent'

const App = () => {
	return <MyComponent name="React" />;
};

export default App;
```

- key값으로 props가 넘어감

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

### children
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

### 비구조화 할당을 사용한 예시

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

