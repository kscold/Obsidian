- [[express]]에서 [[req]](POST 또는 PUT)보낸 req 혹은 [[콜백 함수(Callback Function)]]의 req.body의 [[객체(Object)]]에서 title, body를 추출한다.

- [[express]]를 사용하면서 특별한 설정없이 클라이언트 요청의 body값을 읽으면 [[undefined]]값이 나오게 된다.
- 따라서 req.body의 기본 값으로 undefined로 설정되어 있기 때문에 express.json() 나 express.urlencoded()와 같은 [[미들웨어(Middleware)]]를 사용해서 접근해야 한다.

- [[express]]는 내장으로 [[body-parser]] [[모듈(Module)]]을 가지고 있기 때문에 전역 [[미들웨어(Middleware)]]를 사용하기 편하다.


## 문법

```js
const express = require('express');


const app = express();

app.set('port', process.env.PORT || 8080);

app.use(express.json());
app.use(express.urlencoded({ extended: true }));

app.use(req, res, next) => {
	req.body
}
```


## express.json()

- application/json의 Content-Type에 대해 [[파싱(Parsing)]]해주는 역할을 한다.
- 문서대로 [[미들웨어(Middleware)]]를 사용해주니 이제는 body값에 접근할 수 있다.

- 즉, express.json() [[메서드(Method)]]를 [[미들웨어(Middleware)]]로 선언함에 따라 클라이언트 요청의 body값을 [[서버(Server)]] 내에서 해석 가능한 구문으로 [[파싱(Parsing)]]함과 동시에 req.body값에 할당해주는 역할을 하는 것이다.

## express.urlencoded()

- application/x-www-form-urlencoded의 Content-Type에 대해 파싱해주는 역할을 하며 extended옵션에 따라 다른 라이브러리([[모듈(Module)]])를 사용한다.

-  { extended : true or false } 속성이 있으며 false는 [[노드(Node.js)]]에 기본으로 내장된 queryString 라이브러리 사용하고 true는 따로 설치가 필요한 qs 라이브러리 사용한다.
- qs [[모듈(Module)]]이 훨씬 더 많은 기능을 제공하기 때문에 qs [[모듈(Module)]]사용을 추천한다.