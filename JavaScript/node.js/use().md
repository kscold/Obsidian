
라우터 기능을 만들어 다양한 경로를 처리할 수 있다.
모든 유형의 post, put, get, delete [[메서드(Method)]]에 대해서 반응한다.

## 미들웨어를 사용하지 않은 .use()

지금까지 만든 어플리케이션은 정적파일을 제외한 모든 요청을 index 미들웨어가 처리한다.

```js
app.use(logger())
app.use(serveStatic())
app.use(index)
app.use(error404)
app.use(error)
```

"GET /foo" 로 요청하더라도 index [[미들웨어(Middleware)]]가 동작해서 index.html 파일을 제공하는 것이다. 
물론 에러가 발생할 때는 제외한다.

밑에 코드는 .use() [[메서드(Method)]]를 사용하는 예시

```js
app.use("/", indexController)
app.use("/foo", fooController)
```

특정 주소("/")의 요청이 있을 경우만 설정한 [[미들웨어(Middleware)]](indexController)를 실행하도록 하는 방법이다. 
"/foo" 경로로 요청하면 fooController가 동작한다.

## 애플리케이션 레벨 미들웨어

`app.use()` 및 `app.METHOD()` 함수를 이용해 애플리케이션 미들웨어를의 인스턴스에 바인드한다. 이때 `METHOD`는 미들웨어 함수가 처리하는 요청(GET, PUT 또는 POST 등)의 소문자로 된 HTTP 메소드이다.

다음 예에는 마운트 경로가 없는 미들웨어 함수가 표시되어 있다. 이 함수는 앱이 요청을 수신할 때마다 실행된다.

```javascript
var app = express(); // 혹은 var app = koa()

app.use(function (req, res, next) { // function 혹은 () => {} 콜백함수 사용
  console.log('Time:', Date.now());
  next(); // 다음 미들웨어를 호출
});
```

다음 예에는 `/user/:id` 경로에 마운트되는 미들웨어 함수가 표시되어 있다. 이 함수는 `/user/:id` 경로에 대한 모든 유형의 HTTP 요청에 대해 실행된다.

```javascript
app.use('/user/:id', function (req, res, next) { 
  console.log('Request Type:', req.method); // req의 method
  next();
});
```

다음 예에는 라우트 및 해당 라우트의 핸들러 함수(미들웨어 시스템)이 표시되어 있습니다. 이 함수는 `/user/:id` 경로에 대한 GET 요청을 처리합니다.

```javascript

app.get('/user/:id', function (req, res, next) {
  res.send('USER');
});
```

아래에는 하나의 마운트 경로를 통해 일련의 미들웨어 함수를 하나의 마운트 위치에 로드하는 예가 표시되어 있습니다. 이 예는 `/user/:id` 경로에 대한 모든 유형의 HTTP 요청에 대한 요청 정보를 인쇄하는 미들웨어 하위 스택을 나타냅니다.

```javascript

app.use('/user/:id', function(req, res, next) {
  console.log('Request URL:', req.originalUrl);
  next();
}, function (req, res, next) {
  console.log('Request Type:', req.method);
  next();
});
```

라우트 핸들러를 이용하면 하나의 경로에 대해 여러 라우트를 정의할 수 있습니다. 아래의 예에서는 `/user/:id` 경로에 대한 GET 요청에 대해 2개의 라우트를 정의합니다. 두 번째 라우트는 어떠한 문제도 발생키지 않지만, 첫 번째 라우트가 요청-응답 주기를 종료시키므로 두 번째 라우트는 절대로 호출되지 않습니다.

다음 예에는 `/user/:id` 경로에 대한 GET 요청을 처리하는 미들웨어 하위 스택이 표시되어 있습니다.

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

라우터 미들웨어 스택의 나머지 미들웨어 함수들을 건너뛰려면 `next('route')`를 호출하여 제어를 그 다음 라우트로 전달하십시오. **참고**: `next('route')`는 `app.METHOD()` 또는 `router.METHOD()` 함수를 이용해 로드된 미들웨어 함수에서만 작동합니다.

다음 예에는 `/user/:id` 경로에 대한 GET 요청을 처리하는 미들웨어 하위 스택이 표시되어 있습니다.

```javascript

app.get('/user/:id', function (req, res, next) {
  // if the user ID is 0, skip to the next route
  if (req.params.id == 0) next('route');
  // otherwise pass the control to the next middleware function in this stack
  else next(); //
}, function (req, res, next) {
  // render a regular page
  res.render('regular');
});

// handler for the /user/:id path, which renders a special page
app.get('/user/:id', function (req, res, next) {
  res.render('special');
});
```