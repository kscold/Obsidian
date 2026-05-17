- [[컴포넌트(Component)]]를 페이지에 렌더링하는 역할을 하며, react-dom 모듈을 불러와 사용할 수 있다.

- 이 함수의 첫번째 파라미터는 페이지에 렌더링할 내용을 [[JSX]] 형태로 작성, 두번째 파라미터에는 해당 [[JSX]]를 렌더링할 document 내부 요소를 설정한다. 

```jsx
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';

ReactDOM.render(
	<React.StrictMode>
		<App />
	</React.StrictMode>,
	document.getElementById('root')
);
```

- 여기서는 [[id]]가 root인 요소(unique한 요소) 안에 렌더링을 하도록 설정했다.
- 이 요소는 public/index.html 파일을 열어 보면 있다.