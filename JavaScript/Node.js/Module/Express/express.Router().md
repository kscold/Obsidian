- [[express]]에서 라우트를 관리하는 [[객체(Object)]]이다,

- Router란 Route를 구성하고 코드를 구성하는데 도움을 줄 수 있는 [[모듈(Module)]]이다.
- 라우터를 사용하면 모든 애플리케이션의 라우트를 하나의 파일에 정의하고 그것을 [[express]] 애플리케이션에서 사용할 수 있다.

- 라우터를 사용하여 특정 경로(URL)에 대한 [[HTTP(Hyper Tranfer Protocol)]] 요청을 처리할 수 있다.

- 각 라우트는 단독적으로 작성하고, 웹 애플리케이션에서 필요할 때 조합하여 사용할 수 있다.


## 라우터 객체 HTTP 메서드

- 요청 패스를 라우터 [[객체(Object)]]에 등록할 때 사용하는 [[메서드(Method)]]엔 `get()`, `post()`, `put()`, `delete()`,`all()` 등이 있다.


## 라우터 등록 및 사용방법

- 아래 코드처럼 / 경로에 대한 라우터를 등록한다.

```js
// /routes/index.js
const express = require('express');
const router = express.Router();

router.get('/', function(req, res) {
	res.send('안녕하세요');
});

module.exports = router;
```

- App.js에서 등록한 라우터 정보를 [[모듈(Module)]]로 가져와서 [[객체(Object)]]를 등록한다.

```js
// App.js
const express = require('express');
const app = express();
const router = require('./routes/index');

app.use('/', router);

app.listen(3000);
```


## URL 파라미터를 사용하는 방법

- URL 파라미터는 요청 파라미터([[쿼리스트링(Querystring)]])와 달리 URL 주소의 일부로 들어간다.

- URL 파라미터와 [[쿼리스트링(Querystring)]]는 두 가지 다른 방식으로 [[HTTP(Hyper Tranfer Protocol)]] 요청에 데이터를 전달하는 방법이다.

- 요청의 정보를 보다 효율적으로 전달하고자 할 때, URL 파라미터 또는 [[쿼리스트링(Querystring)]] 중 하나를 사용할 수 있다. 
- [[서버(Server)]]로 전달하고자 하는 데이터의 정보 수준과 개인적인 선택에 따라 어떤 파라미터를 사용할지 결정할 수 있다.

```js
app.get("/users/:id", (req, res) => {
	const id = req.params.id;
	const user = users.find(user => user.id === id);
	res.send(user);
});
```

- 위의 예시에서 `/:id`는 `/users/` 뒤에 오는 값을 [[매개변수(parameter)]]로 처리하겠다는 의미이다.

- 이렇게 지정한 [[매개변수(parameter)]]는 req.params [[객체(Object)]] 안에 들어간다.
- 따라서 `:id`으로 표시된 부분에 담겨 전달된 값은 [[req]].params.id 속성으로 접근할 수 있다.