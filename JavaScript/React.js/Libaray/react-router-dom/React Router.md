- [[리액트(React)]]는 [[SPA(Single Page Application)]]이기 때문에 처음 접속 시 [[HTML(Hyper Text Markup Language)]]을 한번만 받아와서 웹 어플리케이션을 실행시킨다.
- 따라서 기술적으로는 한 페이지만 존재하지만, 사용자가 경험하기에는 여러 페이지가 존재하는 것처럼 느낄 수 있다.

- 리액트 라우터 같은 [[라우팅(Routing)]] 시스템은 사용자의 브라우저 주소창의 경로에 따라 알맞은 페이지를 보여준다.
- 이후 링크를 눌러서 다른 페이지로 이동할 때 서버에 다른 페이지의 [[HTML(Hyper Text Markup Language)]]을 새로 요청하는 것이 아니라, 브라우저의 [[History API]]를 사용하여 브라우저의 주소창의 값만 변경하고 기존에 페이지에 띄웠던 웹 어플리케이션을 그대로 유지하면서 라우팅 설정에 따라 또 다른 페이지를 보여주게 된다.

- 리엑트 라우터를 설치할 때는 react-router-dom을 설치하면 된다.

## [[SPA(Single Page Application)]]를 사용하기에 나오는 문제점

- [[리액트(React)]]는 [[SPA(Single Page Application)]]의 특성을 가지고 있기 떄문에 아래와 같은 다양한 문제가 발생한다.

### 1. 특정 페이지 즐겨찾기 등록 불가

- 화면 전환이 되어도 url 은 고정되어 있기 때문에 내가 원하는 페이지를 특정할 수 없다.

### 2. 뒤로가기 불가 

- 마찬가지의 이유로 해당 SPA 하나에 url 하나이기 때문에 뒤로 가기를 누르면 이전에 보던 다른 웹사이트로 이동하게 된다.

### 3. 새로고침 불가 

- 이 또한 마찬가지 이유로 새로고침을 누를 시 맨 처음의 렌더링 페이지로 이동하게 된다.
- 즉 유저가 보던 페이지가 아닌 처음의 페이지가 나온다.
- 또한 새로고침을 하게 되면 [[SPA(Single Page Application)]]의 특성상 모든 리소스를 다시 로드하게 된다.

### 4. [[SEO(Search Engine Optimization)]] 

- 검색 엔진에 의해 원치 않는 방식으로 색인될 가능성이 있다.

## BrowerRouter

- 동적인 페이지에 적합하다.
- 검색엔진 최적화([[SEO(Search Engine Optimization)]])되어 있다.
- github-pages 배포가 까다롭다.

- 프로젝트에 리액트 라우터를 적용할 때는 react-router-dom에 내장되어 있는 [[<BrowserRouter>]]라는 [[컴포넌트(Component)]]를 사용하여 감싸면 된다.
- 이 [[컴포넌트(Component)]]는 웹 어플리케이션에 [[HTML(Hyper Text Markup Language)]]5의 [[History API]]를 사용하여 페이지를 새로 불러오지 않고도 주소를 변경하고 현재 주소의 경로에 관련된 정보를 리액트 컴포넌트에서 사용할 수 있도록 해준다.

```jsx
import { BrowserRouter } from 'react-router-dom';


const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
	<BrowserRouter>
		<App />
	</BrowserRouter>
);

reportWebVitals();
```

- 위의 코드처럼 BrowerRouter 설정을 통해 사용 [[스코프(Scope)]]를 지정했으면 `<Routes>`와 `<Route>` [[컴포넌트(Component)]]를 통해 주소와 보여줄 [[컴포넌트(Component)]]를 매칭시켜 등록할 수 있다.

```jsx
import React from 'react';
import { Route, Routes } from 'react-router-dom';
import About from './page/About';


const App = () => {
	return (
		<Routes>
			<Route path="/" element={<Home />} />
			<Route path="/about" element={<About />} />
		</Routes>
	);
};

export default App;
```

## **HashRouter**

- 정적인 페이지에 적합( 예) 개인 포트폴리오)하다.
- 검색 엔진으로 읽지 못한다.
- `#`값 때문에 서버가 읽지 못하고 서버가 페이지의 유무를 모른다.
- github-pagers 배포가 간편하다.

## RouterProvider

- [[<RouterProvider>]] [[컴포넌트(Component)]]는 React Router v6.4에 추가된 방식으로 추가적인 기능을 많이 내포하고 있다.
- [[<Suspense>]]를 통하여 로딩화면을 표시하기도 편하며 App.js에 라우팅 설정을 응집화시키지 않고 children 기능을 응용하기 용이하다.


## [[<Link>]]를 사용하여 다른 페이지로 이동

- 리액트 라우터를 사용하는 프로젝트에서는 [[<a>]] 태그를 바로 사용하면 안된다.
- 왜냐하면 a 태그를 클릭하여 페이지를 이동할 때 브라우저에서는 페이지를 새로 불러오게 되기 때문이다.

- [[<Link>]] [[컴포넌트(Component)]] 역시 [[<a>]] 태그를 내부적으로 사용하긴 하지만, 페이지를 새로 불러오는 것을 막고 [[History API]]를 통해 브라우저 주소의 경로만 바꾸는 기능이 내장되어 있다.

```jsx
import React from 'react';
import { Link } from 'react-router-dom';

  
const Home = () => {
	return (
		<div>
			<h1>홈</h1>
			<p>가장 먼저 보여지는 페이지입니다.</p>
			<Link to="/about">소개</Link>
		</div>
	);
};

export default Home;
```

## [[URL 파라미터]]와 [[쿼리스트링(Querystring)]]

- [[URL 파라미터]]는 주로 ID 또는 이름을 사용하여 특정 데이터를 조회할 때 사용하고, [[쿼리스트링(Querystring)]]은 키워드 검색, 페이지네이션, 정렬 방식 등 데이터 조회에 필요한 옵션을 전달할 때 사용한다.