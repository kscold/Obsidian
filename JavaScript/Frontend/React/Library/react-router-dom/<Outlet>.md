- [[React Router]]에서 `<Outlet>` 기능을 제공하는데, 우리는 이를 사용해서 원하는 대로 [[중첩 라우팅]]을 구현할 수 있다.
- [[<BrowserRouter>]]와 [[<RouterProvider>]] 모두 사용가능하다.

## 예시

```jsx
<BrowserRouter>
    <Header />
    <Routes>
        <Route path='/' element={<Main />} />
        <Route path='/mypage' element={<MyPage />} />
    </Routes>
</BrowerRouter>
```

- 하지만 위의 코드는 다음과 같은 상황에서는 좋지 않은 코드일 것이다.
	- 메인페이지와 마이페이지가 존재한다.
	- 마이페이지에서만 사이드 바를 띄우고 싶다.
	- 사이드 바에서 항목을 클릭하면 마이페이지 내부에서 또 다시 라우팅을 하고 싶다.
	- 사이드 바는 고정한다.

- 따라서 웹 페이지의 헤더는 계속 고정해두면 되기 때문에 아래처럼 쉽게 구현할 수 있다.

- 우선 [[<BrowserRouter>]]를 아래와 같이 설정해준다.

```jsx
const AppRouter = () => {
	return (
    	<BrowserRouter>
            <Routes>
            	<Route path = "/" element = {<Test />}>
                    <Route path="testA" element = {<TestA />} />
                    <Route path="testB" element = {<TestB />} />
                </Route>
            </Routes>
        </BrowserRouter>
    );
};

export default AppRouter;
```

- Test 페이지에서 TestA, TestB를 이동했을 때, path에 맞게 TestA 또는 TestB 컴포넌트를 렌더링해줄 것이다.
- TestA, TestB도 아래와 같이 간단하게 작성했다.

```jsx
const TestA = () => {
  return (
    <div>
      <h1>Test 페이지 A</h1>
    </div>
  );
};

export default TestA;
```

```jsx
const TestB = () => {
  return (
    <div>
      <h1>Test 페이지 B</h1>
    </div>
  );
};

export default TestB;
```

- Test.tsx에서는 Outlet을 import해 TestA나 TestB가 들어갈 부분에서 작성하면 된다.
- 위치별로 비교해보면서 실행 화면과 같이 보도록 하겠다.

```jsx
import { Outlet } from 'react-router-dom';

const Test = () => {
  return (
    <>
      <div>
        <h1>Test 페이지</h1>
        <Outlet /> // 1 TestA
        <h2>Test 페이지</h2>
        <Outlet /> // 2 TestB
        <h3>Test 페이지</h3>
        <Outlet /> // 3
      </div>
      <div>
        <h1>Test 페이지</h1>
      </div>
    </>
  );
};

export default Test;
```

- Test 컴포넌트만 렌더링된 화면은 아래와 같다.

![](https://blog.kakaocdn.net/dn/42pFt/btsgDLUuu8f/n6f41k9Tye2KKZpqv8LgTk/img.png)

- /testA로 이동했을 때 화면은 아래와 같다.
- 즉, Outlet이 1번 자리에 있을 때이다.

![](https://blog.kakaocdn.net/dn/wUMee/btsgMbYx5C6/MpVt9vbPPKGpqWYB876Tp1/img.png)

- /testB로 이동했을 때 화면은 아래와 같다.
- Outlet이 2번 자리에 있을 때이다.

![](https://blog.kakaocdn.net/dn/brzARN/btsgJ1hB3mD/NrstqJAXEPiD89iIfkMRok/img.png)

- 따라서 이렇게 원하는 곳에 하위 페이지로 라우팅이 가능하다.