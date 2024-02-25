- 브라우저 History API는 직접적으로 [[리액트(React)]]와 연관된 건 아니지만, [[SPA(Single Page Application)]]에서 자주 쓰이는 react-router-dom처럼, 클라이언트사이드 [[라우팅(Routing)]]을 구현하려면, 자바스크립트의 브라우저 히스토리 API에 대한 이해가 필요하다.

- 따라서 브라우저가 서버로 페이지 전환요청하지 않고, 페이지 전환 및 History API를 이용한 뒤로가기가 가능해야 한다.

## Window.history 속성과 History

- [[Window]] [[객체(Object)]]의 history 읽기 전용 속성은 History 객체(브라우저)로의 참조를 반환한다. 

- History 객체는 브라우저의 세션 기록(현재 페이지를 불러온 탭 혹은 프레임이 방문했던 페이지)을 조작할 때 사용한다.
- 브라우저 세션기록을 조작하기 위해서 History 객체의 참조를 반환하는 Window.history 읽기 전용 속성을 이용하는 것이다.

## 클라이언트 사이드 라우팅만들기

- window.history속성을 이용해 클라이언트 사이드 라우팅을 구현해보고, react-router-dom을 이용한 라우팅도 만들어자.

- 브라우저에는 히스토리에 [[state]]를 저장하는 스택(stack)이 존재한다.

```jsx
// App.js
import React, { Component } from 'react';

class App extends Component {
	
	componentDidMount () { // 첫 렌더링을 마친 후 실행 useEffect(() => {}, [])
		window.onpopstate = function (event) {
		    console.log(`location : ${document.location}, state : ${event.state}`)
		}
	} 
	
	render () {
		return (
			<div>
				<button onClick={()=> window.history.pushState('v1','','/page1')}>
					page1
		        </button>
		        <button onClick={()=> window.history.pushState('v2','','/page2')}>
			        page1
		        </button>
		    </div>
		)
	}
}

export default App;
```

- 먼저 [[ComponentDidMount()]] [[생명 주기(Life Cycle)]]로 [[컴포넌트(component)]]가 마운트되는 시점에 [[이벤트(event)]]를 등록한다.

- window객체에 history의 [[state]]가 pop되면 콘솔에 정보를 출력합니다. 
- onpopstate 뒤로 가기 이용한다.

- 그리고 [[JavaScript/render()|render()]]함수에 버튼 두개를 작성합니다.

- [[onClick]]이벤트가 발생시 state를 push합니다.  
- pushState() 함수의 인자들은 state, title(사파리 제외 역할 없음), url 이렇게 세개입니다.

- 밑의 이미지는 첫 렌더링 화면이다.

![](https://velog.velcdn.com/images%2Fkwonh%2Fpost%2Ff5fec5f0-159c-41a3-9ba3-a6b9a9bdf832%2Fhistory1.png)

- 밑의 이미지는 page 1의 버튼을 눌렸을 때이다.

![](https://velog.velcdn.com/images%2Fkwonh%2Fpost%2Ff99a8246-9387-4c14-b154-04573a3c6b42%2Fhistory2.png)

- 밑의 이미지는 page 2의 버튼을 눌렸을 때이다.

![](https://velog.velcdn.com/images%2Fkwonh%2Fpost%2F16a9bbe9-a7ad-472c-af32-3bf3eae8011f%2Fhistory3.png)

- 이처럼 주소창이 변하는 걸 볼 수 있습다.
- 뒤로가기 버튼을 누르면 onpopstate 이벤트가 발생하면서 로그를 출력한다.
- 이 기능이 클라이언트 사이드에서 라우팅 처리가 되는 것이다.

```jsx
import React, { Component } from 'react';

class App extends Component {
	state = { // 객체 정의
	    pageName : ''
	}
	
	componentDidMount() { // 첫 렌더링 시에 동작
		window.onpopstate = (event) => {
			this.onChangePage(event.state)
		}
	}
	  
	onChangePage = pageName => {
		this.setState({pageName})
	}
	  
	onClick1 = () => {
	    const pageName = 'page1';
	    window.history.pushState(pageName, '', '/page1');
	    this.onChangePage(pageName)  
	}
	
	onClick2 = () => {
	    const pageName = 'page2';
	    window.history.pushState(pageName, '', '/page2');
	    this.onChangePage(pageName)  
	}
	
	render () {
		const {pageName} = this.state;
	    return (
		    <div>
				<button onClick={this.onClick1}>page1</button>
			    <button onClick={this.onClick2}>page2</button>
			    {!pageName && <Home/>} // pageName 없을 때만 홈화면을 렌더링
			    {pageName==='page1'&&<Page1/>}
			    {pageName==='page2'&&<Page2/>}
		    </div>
		)
	}
}

function Home() {            
	return <h2>여기는 홈페이지입니다. 원하는 페이지 버튼을 클릭하세요.</h2>
}

function Page1() {
	return <h2>여기는 Page1입니다.</h2>
}

function Page2() {
	return <h2>여기는 Page2입니다.</h2>
}

export default App;
```

- 위의 코드를 보면 서버에 요청을 보내지않고 클라이언트 사이드에서 사용자의 요청에 따라 페이지가 변화한다.
- 따라서 서버와 통신이 꼭 필요한 기능들을 제외하곤 클라이언트사이드에서 사용자가 원하는 페이지를 보여줄 수 있다.