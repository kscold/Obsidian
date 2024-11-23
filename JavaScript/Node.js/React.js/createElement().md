- [[JSX]] 문법 없이 [[리액트(React)]]의 view 부분을 만들 수 있는 [[메서드(Method)]]이다.

- 또한 이 문법으로 만들 시에 빌드과정을 포함한다.


![[Pasted image 20240402174940.png]]


## App.js에서 렌더링


- 아래 코드는 원래 코드와 비교하여 createElement()로 만든 코드를 가져오는 방법이다.

```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';

import App from './App.jsx';
import './index.css';

  

ReactDOM.createRoot(document.getElementById('root')).render(<App />);
```


```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';

import App from './App.jsx';
import './index.css';

  

ReactDOM.createRoot(document.getElementById('root')).render(React.createElement(App));
// React.createElement(App)으로 코드를 변경
```