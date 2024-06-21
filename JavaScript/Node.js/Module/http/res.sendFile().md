- [[노드(Node.js)]]의 [[express]] 라이브버리의 res [[객체(Object)]]에 정의 되어있는 res.sendFile() [[메서드(Method)]]는 말 그대로 response에 해당 파일을 전송하는 명령어이다.

- 보통은 이 [[메서드(Method)]]를 통해 클라이언트 즉, FE단(view)으로 데이터를 전송하는데 쓰인다.

- 뷰로 연결하는 경우 [[path]].join은 경로를 지정해주는 [[메서드(Method)]]이다.   

## [[path]].join과 같이 사용

- path.join에서는 미리 선언된 `__dirname`을 사용할 수 있다. 
- `__dirname`는 현재 해당 파일이 존재하는 폴더 경로를 뜻한다.  
- 단, [[CommonJS]] [[모듈(Module)]]에서는 별도의 `__dirname` 선언 없이도 바로 사용할 수 있지만, [[ES module]]에서는 별도의 선언이 필요하다.


## 문법

```js
//CommonJS 모듈
const express = require('express');
const path = require('path'); 

//ES 모듈
import express from 'express';
import path from 'path'; 

const __dirname = path.resolve();
```


## 예시

```js
import express from 'express';
import path from 'path'; 

const router = express.Router();
const __dirname = path.resolve();

router.get('*', (req, res) => {  
	res.sendFile(path.join(__dirname, '../client/build/index.html'));
});
```

- 위의 코드처럼 작성하면 해당 [[서버(Server)]]에서 어떤 경로로 요청이 들어오든지 상관없이, 모두 해당 파일이 있는 폴더의 상위 폴더에서 client/build에 있는 index.html을 응답하게 된다.

- 즉, [[SPA(Single Page Application)]]로 연결해줄 때 꼭 써야하는 코드이다.