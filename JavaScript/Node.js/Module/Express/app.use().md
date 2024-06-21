- [[노드(Node.js)]]의 [[express]] [[미들웨어(Middleware)]]를 만드는 [[메서드(Method)]]이다.
- 라우터 기능을 만들어 다양한 경로를 처리할 수 있다.

- 모든 유형의 post, put, get, delete [[메서드(Method)]]에 대해서 반응한다.


## 문법

- app.use 첫번째 인자는 옵션으로 경로가 들어가며 두번째 인자는 [[req]], [[res]] next를 인자로 가지는 [[콜백 함수(Callback Function)]]가 들어간다.

```javascript
const app = express(); // 혹은 const app = koa()

app.use(() => (req, res, next) { // function 혹은 () => {} 콜백함수 사용
	...
	next() // 다음 미들웨어나 작업으로 이돔
});

// 특정 경로에만 미들웨어를 적용
app.use('/경로', () => (req, res, next) { // function 혹은 () => {} 콜백함수 사용
	...
	next() // 다음 미들웨어나 작업으로 이돔
});
```


## [[미들웨어(Middleware)]] 동작 레벨

### 애플리케이션 레벨 미들웨어

- 라우터를 이용하여 어플리케이션(Home logic) 레벨에서 요청처리를 분기해준다.
- app.use() 및 app.METHOD() [[메서드(Method)]]를 이용해 애플리케이션 [[미들웨어(Middleware)]]를의 [[인스턴스(Instance)]]에 바인드한다. 

- METHOD는 [[미들웨어(Middleware)]] 함수가 처리하는 요청(GET, PUT 또는 POST 등)의 소문자로 된 [[HTTP(Hyper Tranfer Protocol)]] [[메서드(Method)]]이다.

- 다음 예시 코드는 마운트 경로가 없는 미들웨어 함수가 표시되어 있다.
- 이 함수는 앱이 요청을 수신할 때마다 실행된다.

```javascript
var app = express(); // 혹은 var app = koa()

app.use(function (req, res, next) { // function 혹은 () => {} 콜백함수 사용
	console.log('Time:', Date.now());
	next(); // 다음 미들웨어를 호출
});
```

- 다음 예시는 `/user/:id` 경로에 마운트되는 미들웨어 함수가 표시되어 있다.
- 이 함수는 `/user/:id` 경로에 대한 모든 유형의 [[HTTP(Hyper Tranfer Protocol)]] 요청에 대해 실행된다.

```javascript
app.use('/user/:id', function (req, res, next) { 
	console.log('Request Type:', req.method); // req의 method
	next();
});
```

- 다음 예시는 라우트 및 해당 라우트의 핸들러 함수(미들웨어 시스템)이 표시되어 있다.
- 이 [[함수(Function)]]는 `/user/:id` 경로에 대한 GET 요청을 처리한다.

```javascript

app.get('/user/:id', function (req, res, next) {
  res.send('USER');
});
```

- 아래에는 하나의 마운트 경로를 통해 일련의 미들웨어 함수를 하나의 마운트 위치에 로드하는 예가 표시되어 있다.
- 이 예는 `/user/:id` 경로에 대한 모든 유형의 HTTP 요청에 대한 요청 정보를 인쇄하는 미들웨어 하위 스택을 나타낸다.

```javascript
app.use('/user/:id', function(req, res, next) {
	console.log('Request URL:', req.originalUrl);
	next();
}, function (req, res, next) {
	console.log('Request Type:', req.method);
	next();
});
```

- 라우트 핸들러를 이용하면 하나의 경로에 대해 여러 라우트를 정의할 수 있다.

- 아래의 예시서는 `/user/:id` 경로에 대한 GET 요청에 대해 2개의 라우트를 정의한다.
- 두 번째 라우트는 어떠한 문제도 발생키지 않지만, 첫 번째 라우트가 요청-응답 주기를 종료시키므로 두 번째 라우트는 절대로 호출되지 않는다.

- 다음 예시는 `/user/:id` 경로에 대한 GET 요청을 처리하는 미들웨어 하위 스택이 표시되어 있다.

```javascript
app.get('/user/:id', function (req, res, next) {
	console.log('ID:', req.params.id);
	next();
}, function (req, res, next) {
	res.send('User Info');
});

// handler for the /user/:id path, which prints the user ID
app.get('/user/:id', function (req, res, next) {
	res.end(req.params.id);
});
```

- 라우터 미들웨어 스택의 나머지 미들웨어 함수들을 건너뛰려면 `next('route')`를 호출하여 제어를 그 다음 라우트로 전달한다.
- `next('route')`는 `app.METHOD()` 또는 `router.METHOD()` 함수를 이용해 로드된 미들웨어 함수에서만 작동한다.

- 다음 예시는 `/user/:id` 경로에 대한 GET 요청을 처리하는 미들웨어 하위 스택이 표시되어 있다.

```javascript
app.get('/user/:id', function (req, res, next) {
	// if the user ID is 0, skip to the next route
	if (req.params.id == 0) next('route');
	// otherwise pass the control to the next middleware function in this stack
	else next(); //
	}, function (req, res, next) {
		// render a regular page
		res.render('regular');
	}
);

// handler for the /user/:id path, which renders a special page
app.get('/user/:id', function (req, res, next) {
	res.render('special');
});
```


### 라우터 레벨 미들웨어

- 어플리케이션의 분기처리에서 넘어온 요청을 세부적으로 구성해준다.

- [[미들웨어(Middleware)]]를 사용하지 않은 app.use() 사용이다.
- 아래 코드처럼 index.js에 app.use()를 선언할 시에는 모든 요청은 index 미들웨어가 처리한다.

```js
app.use(logger())
app.use(serveStatic())
app.use(index)
app.use(error404)
app.use(error)
```

- "GET /foo" 로 요청하더라도 index [[미들웨어(Middleware)]]가 동작해서 index.html 파일을 제공하는 것이다.
- 물론 에러가 발생할 때는 제외한다.

- 밑에 코드는 .use() [[메서드(Method)]]를 사용하는 예시이다.

```js
app.use("/", indexController)
app.use("/foo", fooController)
```

- 특정 주소("/")의 요청이 있을 경우만 설정한 [[미들웨어(Middleware)]](indexController)를 실행하도록 하는 방법이다. 
- "/foo" 경로로 요청하면 fooController가 동작한다.

-  밑의 코드는 app.use()가 [[미들웨어(Middleware)]]를 통해 라우팅 처리받은 것은 동일하게 Router를 통해 진행한다.

```javascript
const router = express.Router()

router.get('/', (req, res) => {
    //res.send('THIS IS entities')
    Model.findAll()
    .then(list=>{
        console.log(list)
        res.sendStatus(200)
    })
    .catch(err => console.error('ERROR FOUND : ', err))
}
```

- 위에서 미들웨어를 통해(= app.use) 받아온 요청에서 추가 url이 없을 경우 get('/')(`*`루트 디렉토리 요청으로 처리)하여 추가적인 미들웨어를 생성하지 않는다.

- 해당 요청에서 처리받은 결과를 res인자로 받아 최종적으로 화면을 구성하는 logic을 구성한다.
- app.use를 통해 어플리케이션 레벨로부터 받은 요청을 Router 처리해주며, 이 처리과정이 라우터 레벨에서의 처리이다.
