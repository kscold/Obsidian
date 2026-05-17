- [[React Router]] 라이브러리의 [[<BrowserRouter>]] 방식에 사용된다.

- 현재 브라우저의 location(window.href.location 정보를 가져온다.)상태에 따라 다른 element를 렌더링한다.
- Route.element: 조건이 맞을 때 렌더링할 element, <Element />의 형식으로 전달된다.
- Route.path: 현재 path값이 url과 일치하는지 확인해 해당 url에 매칭된 element를 렌더링해준다.