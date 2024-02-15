- [[<a>]]태그의 href로 이동하면 상태 값을 잃고 속도가 저하된다.  
- [[리액트(React)]]는 단일 url을 가지고 [[SPA(Single Page Application)]]으로 사이트를 표현하기 때문에 하나의 [[HTML]] 페이지와 애플리케이션 실행에 필요한 JavaScript와 CSS 같은 모든 자산을 로드하는 애플리케이션이다.

- 해당 이유로 페이지를 새로 불러오게 되면 앱이 지니고 있는 상태가 초기화되고, 렌더링 된 [[컴포넌트(component)]]도 모두 사라지고 새로 렌더링을 해야 한다.

- 상태([[state]]) 유지와 속도의 효율성을 위해 새로운 페이지를 불러오는 대신 업데이트하는 방식으로 구현해야 한다.

- [[<a>]] 태그의 href는 페이지를 이동시킬 때 페이지를 새로 불러오게 된다.
- 따라서 상태 값이 유지되지 못하고 속도도 저하된다.

- 반면에, Link [[컴포넌트(component)]]는 HTML5 History API를 사용하여 브라우저의 주소만 바꿀 뿐, 페이지를 새로 불러오지는 않는다.
- 따라서 [[리액트(React)]]에서는 Link 컴포넌트 사용을 권장한다.

## 예시

```jsx
import React from 'react';
import { Route, Link } from 'react-router-dom';
import About from './About';
import Home from './Home';

const App = () => {
    
	return (
        <div>
	        <ul>
		        <li>
			        <Link to="/">홈</Link>
			    </li>
				<li>
					<Link to="/about">소개</Link>
	            </li>
	        </ul>
	        
	        <hr />
	        
	        <Route path="/" exact={true} component={Home} />
	        <Route path="/about" component={About} />
	    </div>
	);
};

export default App;
```
