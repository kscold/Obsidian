- [[<a>]]태그의 href로 이동하면 상태 값을 잃고 속도가 저하된다.  
- [[리액트(React)]]는 단일 url을 가지고 [[SPA(Single Page Application)]]으로 사이트를 표현하기 때문에 하나의 [[HTML(Hyper Text Markup Language)]] 페이지와 애플리케이션 실행에 필요한 자바스크립트와 CSS 같은 모든 자산을 로드하는 애플리케이션이다.

- 해당 이유로 페이지를 새로 불러오게 되면 앱이 지니고 있는 상태가 초기화되고, 렌더링 된 [[컴포넌트(Component)]]도 모두 사라지고 새로 렌더링을 해야 한다.

- 상태([[state]]) 유지와 속도의 효율성을 위해 새로운 페이지를 불러오는 대신 업데이트하는 방식으로 구현해야 한다.

- [[<a>]] 태그의 href는 페이지를 이동시킬 때 페이지를 새로 불러오게 된다.
- 따라서 상태 값이 유지되지 못하고 속도도 저하된다.
- 반면에, Link [[컴포넌트(Component)]]는 HTML5 History API를 사용하여 브라우저의 주소만 바꿀 뿐, 페이지를 새로 불러오지는 않는다.
- 따라서 [[리액트(React)]]에서는 [[React Router]] 라이브러리의 Link [[컴포넌트(Component)]] 사용을 권장한다.

- `<Link>` 가 쓰이는 경우는 주로 사용자가 클릭해서 페이지를 이동하도록 할 때 사용 대부분의 경우 Link 만으로도 충분할 때 사용한다.
- 예를 들면 하이퍼링크 텍스트, 페이지를 이동하는 버튼, 이미지 등이 있다.

## `<Link>`대신 다른 이동 컴포넌트를 사용하는 경우

### [[<Navigate>]]

- 특정 경로에서 렌더링 시점에 다른 페이지로 이동시키고 싶을 때는 [[<Navigate>]]를 사용한다.

- 쇼핑몰의 회원 전용 페이지에 로그인없이 들어와서 로그인 페이지로 리다이렉트하는 경우이다.
- 쇼핑몰의 상품 상세 페이지에서 제품이 품절되었거나 삭제되어서 다른 페이지로 이동시키는 경우이다.

#### [[useNavigate()]]

- 특정한 코드의 실행이 끝나고 나서 페이지를 이동시키고 싶을 때는 [[useNavigate()]]를 사용한다.

- 쇼핑몰에서 장바구니에 담기를 눌렀을 때 리퀘스트를 보내고 장바구니 페이지로 이동시키는 경우이다.
- 쇼핑몰에서 결제하기 버튼을 누르고 나서 모든 결제가 완료된 후에 페이지를 이동시키는 경우이다.
- 리다이렉트된 로그인 페이지에서 로그인을 완료한 후에 처음 진입했던 페이지로 돌아가는 경우이다.


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
