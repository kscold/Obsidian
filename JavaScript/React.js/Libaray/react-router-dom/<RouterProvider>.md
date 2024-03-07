- 지금까지 [[React Router]] 라이브러리에서 사용했던 [[<BrowserRouter>]]라우터 방식말고 React Router v6.4에 추가된 방식이다.

## RouterProvider와 CreateBrowserRouter

 - 이제 RouterProvider와 CreateBrowserRouter를 사용했을 때, 폴더구조는 아래와 같다.

![](https://blog.kakaocdn.net/dn/bgLbGP/btr3aKG3pYT/AGnI86CE1WtcMnhSjl6aq0/img.png)

- 기존 방식과는 다르게 새로운 컴포넌트를 만들어서 route 링크 들만 따로 분리를 시켜서 관리하도록 만든다.
- 기존 구성에서 중첩 되어있는 경로는 children을 이용해서 중첩을 사용할 수 있다.

```js
// RouterInfo.jsx
import Layout from '../Layout'
import Main from '../pages/Main';
import PageA from '../pages/PageA';
import PageB from '../pages/PageB';
import PageC from '../pages/PageC';

export const RouterInfo = [ // 패스 설정파일을 새로 만듬
  {
    path: "/",
    element: <Layout />,
    children: [
      {
        index: true,
        element: <Main />,
        label: 'main' // 경로를 설정하는데 /를 붙이지 않고 씀
      },
      {
        path: "/pageA",
        element: <PageA />,
        label: 'A'
      },
      {
        path: "/pageB",
        element: <PageB />,
        label: 'B'
      },
      {
        path: "/pageC",
        element: <PageC />,
        label: 'C'
      },
    ]
  },
]
```

- 우선 기존의 방식처럼 BrowserRouter로 감싸지 않는다.
- RouterProvider를 이용해서 router [[props]]에 구성요소들을 전달하고 활성화한다.

```js
import { createBrowserRouter, RouterProvider, } from 'react-router-dom';
import { RouterInfo } from './util/router';

const RouterObject = createBrowserRouter(RouterInfo) // 패스 데이터를 받아옴
//CreateBrowserRouter로 경로 지정

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <RouterProvider router={RouterObject} />
  </React.StrictMode>
);
```

- 아래 예시 코드처럼 조금 더 업그레이드하여 패스 데이터를 받아올 때 [[lazy()]] 문법을 사용할 수 있다.

```jsx
import { createBrowserRouter } from 'react-router-dom';
import { Suspense, lazy } from 'react';

const Loading = <div className="bg-red-700">Loading...</div>;

const Main = lazy(() => import('../pages/MainPage'));
const About = lazy(() => import('../pages/AboutPage'));

const root = createBrowserRouter([
	// ReactRouter설정으로 객체형식으로 패스를 설정
	{
		path: '', // 경로 설정
		element: (
			<Suspense fallback={Loading}>
				<Main />
			</Suspense>
		),
	},
	{
		path: 'about',
		element: (
			<Suspense fallback={About}>
				<About />
			</Suspense>
		),
	},
]);


export default root;
```


## [[<Outlet>]]

- 또한 `<RouterProvider>`와 CreateBrowserRouter를 사용하였을 때 패스에서 children을 설정하면 uri 패스에 따라 [[<Outlet>]]를 통해 [[children]] [[컴포넌트(component)]]를 설정할 수 있다.

```jsx
import { createBrowserRouter } from 'react-router-dom';
import { Suspense, lazy } from 'react';


const Loading = <div className="bg-red-700">Loading...</div>;

const Main = lazy(() => import('../pages/MainPage'));

const About = lazy(() => import('../pages/AboutPage'));

const TodoIndex = lazy(() => import('../pages/todo/IndexPage'));

const TodoList = lazy(() => import('../pages/todo/ListPage'));

  

const root = createBrowserRouter([ // ReactRouter설정으로 객체형식으로 패스를 설정
	{
		path: '', // 경로 설정
		element: (
			<Suspense fallback={Loading}>
				<Main />
			</Suspense>
		),
	},
	{
		path: 'about',
		element: (
			<Suspense fallback={Loading}>
				<About />
			</Suspense>
		),
	},
	{
		path: 'todo',
		element: (
			<Suspense fallback={Loading}>
				<TodoIndex />
			</Suspense>
		),
		children: [ // 하위 라우팅을 분기 <Outlet>으로 접근가능
			{
				path: 'list',
				element: (
					<Suspense fallback={Loading}>
						<TodoList />
					</Suspense>
				),
			},
		],
	},
]);


export default root;
```


```jsx
import React from 'react';
import { Outlet } from 'react-router-dom';
import BasicLayout from '../../layouts/BasicLayout';

  

const IndexPage = () => {
	return (
		<BasicLayout>
			<div className="w-full flex m-2 p-2">
				<div className="text-xl m-1 p-2 w-20 font-extrabold text-center underline">
					LIST
				</div>
				<div className="text-xl m-1 p-2 w-20 font-extrabold text-center underline">
					ADD
				</div>
				
				<div className="flex flex-wrap w-full">
					<Outlet /> {/* todo/list element로 연결됨 */} 
				</div>
			</div>
		</BasicLayout>
	);
};
  

export default IndexPage;
```