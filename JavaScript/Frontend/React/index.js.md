- 리엑트에서 가장 상위에 존재하는 코드로 존재하는 가장 상위의 [[DOM(Document Object Model)]]요소로 id가 root인 div 요소를 렌더링 시킨다.

- public/index.html
```html
<html lang="en">
	<head>
		<meta charset="utf-8" />
		<meta name="viewport" content="width=device-width, initial-scale=1" />
		<link rel="icon" href="%PUBLIC_URL%/favicon.ico" />
		<title>React App</title>
	</head>
	<body>
		<div id="root"></div>
	</body>
</html>
```

- src/index.js
```jsx
(...)
 ReactDOM.render(<App />, documentById('root'));
```

 - src/index.js 파일 중에는 id가 root 요소에 리액트 컴포넌트를 렌더링하라는 코드가 있다.
- HTML에서 id를 사용하여 DOM에 이름을 다른 것처럼 리액트 프로젝트 내부에서 DOM에 이름을 다는 방법은 [[ref]]를 사용하면 된다.