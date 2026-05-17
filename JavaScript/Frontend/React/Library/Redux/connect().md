- [[Context.Provider]] [[컴포넌트(Component)]]를 이용하여 [[리액트(React)]] [[컴포넌트(Component)]]가 [[스토어(Store)]]에 access(접근)할 권한을 얻게 되었다면 [[스토어(Store)]]에 저장되어 있는 [[state]]를 이용할 수 있다.

- 각 컴포넌트들이 [[state]]를 이용하기 앞서서 [[스토어(Store)]]를 이용하기 위해서는 [[컴포넌트(Component)]]들을 [[스토어(Store)]]에 연결(connect)시켜야 한다. 
- [[state]]는 [[스토어(Store)]]에 저장되어 있기 때문이다.

- 따라서 [[컴포넌트(Component)]]를 [[스토어(Store)]]에 연결(connect)시킬 때 connect()를 사용한다.

- connect()와 [[mapStateToProps]], [[mapDispatchToProps]]를 사용하는 방법 대신 [[useSelector()]]를 사용할 수 있다.


## 문법

```null
connect(mapStateToProps, mapDispatchToProps)(Home);
```

### [[mapStateToProps]]

- store로부터 state를 가져와서 컴포넌트의 props로 보내게 해주는 함수이다
### [[mapDispatchToProps]]

- [[dispatch()]]를 [[props]]로 보낼 수 있다.
### Home

- 취득한 데이터를 [[props]]로 사용하고 싶은 [[컴포넌트(Component)]]를 지정한다.

## 예시

- 아래 Home 컴포넌트(Home.js)가 실질적으로 To-do(state)들을 렌더링하기 때문에 Home 컴포넌트를 [[스토어(Store)]]와 연결시킨다.

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

export default connect(mapStateToProps, mapDispatchToProps)(Home);
```