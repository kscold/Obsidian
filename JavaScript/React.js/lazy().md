- 리엑트에서 [[컴포넌트(component)]] 파일을 코드의 최상단에 [[import]]로 정의하고 동적으로 불러오기를 사용하면 에러가 발생한다.

- 따라서 [[컴포넌트(component)]]를 동적으로 불러오기 위해서는 React.lazy()를 사용해야 한다.
- React.lazy() [[메서드(Method)]]를 사용하면 동적 가져오기를 사용하여 구성 요소 수준에서 React 애플리케이션을 쉽게 코드 분할할 수 있다.

## React.lazy를 사용하는 이유

- 일반적으로 규모가 큰 React 애플리케이션은 많은 요소, 라이브러리 등으로 구성된다.
- 필요할 때만 애플리케이션의 다른 부분을 로드하려고 노력하지 않으면 사용자가 첫 페이지를 로드하는 즉시 대규모 단일 자바스크립트 번들이 사용자에게 전송된다.

- 이는 페이지 성능에 상당한 영향을 줄 수 있다. 
- React.lazy() [[메서드(Method)]] 요소들을 손쉽게 개별 자바스크립트 청크로 분리하는 기본 제공 방법을 제공한다. 

## 문법

```jsx
const About = React.lazy(() => import('./About'));
```

- React.lazy()는 import() 구문을 반환하는 [[콜백 함수(Callback Function)]]를 인자로 받는다.

- 동적으로 불러오는 컴포넌트 파일에는 반드시 지켜줘야하는 두 가지 규칙이 있다.
	1. React [[컴포넌트(component)]]를 포함해야 합니다.
	2. default export를 가진 [[컴포넌트(component)]]여야 한다.

## [[React Router]]와 함께 사용

```jsx
return (
	<Router>
		<Suspense fallback={<div>Loading...</div>}>
		    <Routes>
		        <Route path="/" element=<About/>} />
		    </Routes>
		</Suspense>
	</Router>
);
```

- React.lazy()는 React 공식문서에 따르면, `<Router>` 바로 아래에 [[<Suspense>]]를 위치시키고, Route로 보여줄 컴포넌트들을 React.lazy로 불러오는 것을 권장하고 있다.

## 예시

- 아래 코드는 React.lazy()를 사용한 아주 간단한 예시 코드이다. 

```jsx
import React, { Suspense } from 'react';
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';

function App() {
	const About = React.lazy(() => import('./About')); // lazy 메서드를 통해 동적으로 불러옴
	
	return (
		<Router>
		  <Suspense fallback={<div>Loading...</div>}>
			  <Routes>
				  <Route path="/" element=<About/>} />
		      </Routes>
	      </Suspense>
		</Router>
	);
}
```
