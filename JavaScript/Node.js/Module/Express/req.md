- [[노드(Node.js)]]에서 [[express]]에서 사용되는 요청 [[객체(Object)]]이다.


## req의 [[메서드(Method)]]

### req.app

- req 객체를 통한 app 객체로의 접근이다. 
- 예를 들어 req.app.get('port')와 같은 식으로 사용할 수 있다.
### req.cookies

- cookie-parser [[미들웨어(Middleware)]]가 만드는 요청의 [[쿠키(Cookie)]]를 해석한 [[객체(Object)]]이다.
### req.signedCookies

- 서명된 쿠키들은 req.cookies 대신 여기에 담긴다.
### req.get(헤더 이름) 

- 헤더의 값을 가져온다. 
- req.get('Content-type') 형식으로 사용한다.
### req.[[req.body]]

- body-parser [[미들웨어(Middleware)]]가 만드는 요청의 본문(body)을 해석한 객체이다.
- POST 방식으로 넘어오는 데이터를 담는다.
### [[req.params]]

- 라우트 매개변수**에 대한 정보가 담긴다.
### req.query

- GET방식으로 넘어오는 데이터, [[쿼리스트링(Querystring)]]의 정보가 담긴다. 
### req.route

- 현재 라우트에 관한 정보. 디버깅용이다.
### req.headers

- [[HTTP(Hyper Tranfer Protocol)]]의 Header 정보를 가지고 있다.
### req.accepts(`[types]`)

- 클라이언트가 해당하는 타입을 받을 수 있는지 확인하는 간단한 메서드이다.
### req.ip

- 요청의 ip 주소를 담는다.
### req.path

- 클라이언트가 요청한 경로. 프로토콜, 호스트, 포트, 쿼리스트링을 제외한 순수 요청 경로다.
### req.host

- 요청 호스트 이름을 반환하는 간단한 메서드. 조작될 수 있으므로 보안 목적으로는 사용하면 안된다.
### req.xhr

- 요청이 [[Ajax(Asynchronous JavaScript and XML)]] 호출로 시작되었다면 true를 반환하는 프로퍼티이다.
### req.protocol

- 현재 요청의 프로토콜을 구분한다.
- http, https 등인지 구분한다.
### req.secure

- 현재 요청이 보안 요청(ssl) 이면 true를 반환한다.
### req.url (req.originalUrl)

- URL 경로와 쿼리 스트링을 반환한다.
- 원본 요청을 logging하는 목적으로 많이 쓰인다.
### req.acceptedLanguages

- 클라이언트가 선호하는 자연어 목록을 반환한다.
- header에서 파싱하면 다국어를 지원한는 어플리케이션이라면 초기 언어 선택에 도움을 줄 수 있다.


## req [[객체(Object)]] [[메서드(Method)]] 사용 예시

### req.query

```js
// GET /search?q=tobi+ferret
console.dir(req.query.q)
// => 'tobi ferret'

// GET /shoes?order=desc&shoe[color]=blue&shoe[type]=converse
console.dir(req.query.order)
// => 'desc'

console.dir(req.query.shoe.color)
// => 'blue'

console.dir(req.query.shoe.type)
// => 'converse'

// GET /shoes?color[]=blue&color[]=black&color[]=red
console.dir(req.query.color)
// => ['blue', 'black', 'red']
```

### req.body

```js
var express = require('express')

var app = express()

app.use(express.json()) // for parsing application/json
app.use(express.urlencoded({ extended: true })) // for parsing application/x-www-form-urlencoded

app.post('/profile', function (req, res, next) {
  console.log(req.body)
  res.json(req.body)
})
```

- 클라이언트에서 요청할 때 GET 방식으로 요청할지, 아니면 POST 방식으로 요청할지 모르는 경우가 있을 수도 있다.
- 이럴 때는 다음과 같은 방식으로 두 가지 요청 파라미터를 모두 검사할 수 있다.

```js
const paramId = req.body.id || req.query.id;
```

### req.get(field), req.header(name)

```js
req.get('Content-Type')
// => "text/plain"

req.get('content-type')
// => "text/plain"

req.get('Something')
// => undefined
```
