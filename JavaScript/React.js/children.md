- children은 [[리액트(React)]] [[컴포넌트(component)]] 태그 사이의 내용을 보여주는 [[props]]이다.

## 예시

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

// >> 안녕하시요, 제 이름은 기본 이름입니다.
// >> children 값은 리액트입니다.
```
