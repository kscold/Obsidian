- [[리덕스(Redux)]]에서  사용되는 [[connect()]]의 첫번째 인자로 들어가는 함수이다.

- mapStateToProps는 [[스토어(Store)]]로부터 [[state]]를 가져와서, [[컴포넌트(Component)]]의 [[props]]로 [[state]]를 보내주는 역할을 한다.

- 즉, mapStateToProps를 사용한다는 것은, [[스토어(Store)]]로부터 데이터([[state]])를 가져와서 그 데이터를 [[컴포넌트(Component)]]의 [[props]]에 넣는다는 뜻이다.


## 문법

### state

- [[스토어(Store)]]로부터 온 [[state]]이다.
### ownProps

- 생략가능하다.
- [[컴포넌트(Component)]]가 현재 가지고 있는 모든 [[props]]를 보여준다.


## 예시

- 중요한 것은, mapStateToProps에서 return된 값이 [[컴포넌트(Component)]]의 [[props]]에 추가된다는 점이다.

![](https://velog.velcdn.com/images%2Fiamhayoung%2Fpost%2F0bbfdb57-d120-46ce-b04c-4a711de5915d%2Fimage.png)

- 위의 그림을 보면, mapStateToProps 함수에서 text: "hey"라는 값을 가진 object(객체)를 return시켰다.  

- 그리고 [[connect()]]에서 연결했었던 Home [[컴포넌트(Component)]]의 [[props]]를 확인한 결과 `text: "hey"`라는 값을 받고 있는 것이 보인다.

- 그 외의 값은 [[React Router]]로부터 받은 [[props]]이다.

- 여기서 알 수 있는 것은, mapStateToProps [[함수(Function)]]에서 [[스토어(Store)]]로부터 가져온 [[state]]를 return시킨다면 컴포넌트에서 [[state]]를 [[props]]로 받아서 사용할 수 있다는 것이다.


```jsx
// routes/Home.js
import React, { useState } from "react";
import { connect } from "react-redux";

function Home() {
	const [text, setText] = useState("");
	
	function onSubmit(event) {
	    event.preventDefault();
	    console.log(text);
	    setText("");
	}
	
	function onChange(event) {
	    setText(event.target.value);
	}
	
	return (
		<>
		    <h1>To-do List</h1>
		    <form onSubmit={onSubmit}>
			    <input
				    onChange={onChange}
			        type="text"
			        value={text}
			        placeholder="✍️Write To-do..."
			    />
	        <button>✚</button>
		    </form>
			<ul></ul>
	    </>
	);
}

function mapStateToProps(state) {
	return {
		toDos: state
	};
}

function mapDispatchToProps() {}

export default connect(mapStateToProps, mapDispatchToProps)(Home);
```