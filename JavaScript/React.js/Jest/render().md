- [[Jest]]에서 [[컴포넌트(component)]]가 렌더링되고 있는지에 대한 테스트를 작성할 때 사용한다.

- render()는 특정 컴포넌트를 렌더링하는 [[메서드(Method)]]이다.

- [[테스트]]는 컴포넌트를 렌더링하여 heading역할을 하는 요소를 받아온다. 
- 그리고 해당 엘리먼트가 화면에 존재하는지 검증하면 테스트가 종료된다.

- yarn test의 경우 .test. 파일만 실행시키기 떄문에 render()로 가져오는 [[컴포넌트(component)]]를 app.js 혹은 app.tsx로 생각해야 한다.

## 예시

```js
// App.test.js
import { render, screen } from '@testing-library/react';

test('<App /> 렌더링시 / 경로로 렌더링 되나요?', async () => {
    render(<App />);
    
    const headingEl = screen.getByRole('heading');
    expect(headingEl).toBeInTheDocument();
  });
```

- render()로 [[컴포넌트(component)]]를 렌더링 시키고 [[screen]]으로 요소에 접근한다.