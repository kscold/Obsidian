- Koa나 [[express]]의 Context는 [[HTTP(Hyper Tranfer Protocol)]] 요청(request), 응답(response) [[객체(Object)]]를 단일 [[객체(Object)]]로 캡슐화하여 웹 애플리케이션 및 API 작성에 유용한 많은 [[메서드(Method)]]를 제공한다.

- Context는 [[HTTP(Hyper Tranfer Protocol)]] 요청마다 생성되며 아래와 같은 형태로 [[미들웨어(Middleware)]]에서 참조된다.

- [[express]] 라이브러리의 [[req]], [[res]] [[객체(Object)]]와 거의 유사하다.

## 문법

```js
app.use(async ctx => ({ // 미들웨어로 비동기 콜백 함수를 사용
	ctx, // context 
	ctx.request, // Koa Request
	ctx.response // Koa Response 
});
```

### Request 접근자

```js
ctx.header 
ctx.headers 
ctx.method 
ctx.method= 
ctx.url 
ctx.url= 
ctx.originalUrl 
ctx.origin
ctx.href
ctx.path
ctx.path= 
ctx.query 
ctx.query= 
ctx.querystring
ctx.querystring= 
ctx.host 
ctx.hostname
ctx.fresh 
ctx.stale 
ctx.socket
ctx.protocol
ctx.secure 
ctx.ip 
ctx.ips
ctx.subdomains
ctx.is() 
ctx.accepts()
ctx.acceptsEncodings()
ctx.acceptsCharsets()
ctx.acceptsLanguages()
ctx.get()
```

### Response 접근자

```js
ctx.body 
ctx.body= 
ctx.status 
ctx.status= 
ctx.message 
ctx.message= 
ctx.length= 
ctx.length 
ctx.type= 
ctx.type 
ctx.headerSent 
ctx.redirect() 
ctx.attachment() 
ctx.set() 
ctx.append()
ctx.remove() 
ctx.lastModified= 
ctx.etag=
```


## ctx.req, ctx.res

- Express의 request와 response [[객체(Object)]]를 의미한다.

## ctx.request, ctx.response

- Koa의 request와 response [[객체(Object)]]를 의미한다.

- Koa.request와 Koa.response의 객체를 출력하면 아래와 같다.

```json
// Request
{ 
	method: 'GET', 
	url: '/hello',
	header: { 
		'user-agent': 'vscode-restclient',
		'accept-encoding': 'gzip, deflate',
		host: 'localhost:3002', 
		connection: 'close' 
	} 
}

// Response
{ 
	status: 200, 
	message: 'OK', 
	header: [Object: null prototype] { 
		'content-type': 'text/plain; charset=utf-8',
		'content-length': '11' 
	},
	body: 'Hello World' 
}
```

