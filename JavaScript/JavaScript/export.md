- [[ES module]]의 [[키워드(Keyword)]]로써 [[CommonJS]]처럼 세부적인 설정을 변경할 수 없다.

- [[리액트(React)]]로 개발할 때 하나의 파일에 하나의 [[컴포넌트(Component)]]만 있다면 보통 해당 컴포넌트 명으로 export한다.


## export default

- 하나의 [[모듈(Module)]]에는 대개 하나의 export default가 존재한다.
- export default로 모듈을 내보낸다면 export한 이름과 상관없이 원하는 이름으로 [[import]]가 가능하다.

```javascript
// Test.js
export default function Test() {
  console.log("hi");
}
```


```javascript
// App.js
import { useEffect } from "react";
import Testing from "./Test.js";	// Test로 내보냈지만 내가 원하는 이름(Testing)으로 가져올 수 있다.

function App() {

  useEffect(() => {
    Testing();         // hi가 출력됨
  }, []);

  return <div></div>;
}

export default App;
```

## named export

- named export의 경우 반드시 export한 이름으로만 import할 수 있다.
- 반드시 [[import]]할 때 중괄호로 가져와야한다.

```javascript
// Test.js
export default function Test() {
  console.log("hi");
}


const User = ["John", "Ahn", "Kim"];
export { User }; // User로 내보낸다.
```

```javascript
// App.js
import { useEffect } from "react";
import Testing from "./Test.js";
import { User } from "./Test.js";
// 반드시 User로 import 해야한다.
function App() {
  useEffect(() => {
    Testing();         // hi가 출력됨
	console.log(User); // ["John", "Ahn", "Kim"] 이 출력됨
  }, []);

  return <div></div>;
}

export default App;
```

- 만약 다음과 같이 UserTest로 import하게 된다면 아래와 같이 컴파일에 실패하고 UserTest가 Test.js에 존재하지않다고 알려준다.

```javascript
// 만약 User가 아닌 UserTest로 import를 시도한다면
import { UserTest } from "./Test.js";

function App() {
  console.log(UserTest);

  return <div></div>;
}

export default App;
```

![](https://velog.velcdn.com/images/navyjeongs/post/0b3dd949-6fd3-4809-b5b6-86b29c594a36/image.PNG)
