

### 바닐라 HTML에서 이벤트

- [[이벤트(event)]]를 사용할때 [[onClick]]의 바닐라 버전은 아래와 같이 onclick = ""로 사용한다.
```html
<button onclick="alert('executed')">Click</button>
```


### 리엑트에서 이벤트
- 리액트의 이벤트 시스템은 웹 브라우저의 HTML 이벤트와 인터페이스가 돌일하기 때문에 사용법이 거의 비슷하다.([[JSX]] 이용한다.)
- onClick={} 형식으로 사용한다.

```jsx
import React, { useState } from 'react';

const Say = () => {
	const [message, setMessage] = useState('');
	const onClikEnter = () => setMessage('안녕하세요!');
	const onClikLeave = () => setMessage('안녕히 가세요!');
	
	const [color, setColor] = useState('black');
	
	return (
		<div>
			<button onClick={onClikEnter}>입장</button>
			<button onClick={onClikLeave}>퇴장</button>
		</div>
	)
};

export default Say;
```