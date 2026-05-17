- [[노드(Node.js)]]에서 writeHead()는 [[HTTP(Hyper Tranfer Protocol)]] response의 header를 설정하기 위한 [[HTTP(Hyper Tranfer Protocol)]] [[모듈(Module)]]의 [[메서드(Method)]]이며 두 개의 인자를 받는다.


## 문법

- response.writeHead(statusCode, headers)

### statusCode

- http response의 상태코드를 특정한다.
- 서버로부터 성공적인 응답을 받은경우 200을 할당하고, Not found 오류의 경우 404를 할당한다.

### headers 

- http response header를 나타내는 key-value 쌍이 담긴 객체이다.
- 인자를 전달하지 않을 경우 기본값으로 설정된다.

```js
res.writeHead(200, {
	'Content-Type': 'text/html',
	'Cache-Control': 'no-cache'
});
```

## headers의 종류

- Content-Type : response body의 MIME을 특정한다.
- Content-Length :  response body의 길이를 특정합니다.(bytes)
- Cache-Control : 클라이언트가 response를 어떻게 캐싱할지 특정합니다.
- Location : 클라이언트가 리디릭션하기 위해 지정해야하는 url을 특정하기 위해 사용됩니다.
- Set-Cookie : 클라이언트의 브라우저에서 쿠키를 설정하기 위해 사용됩니다.
- Access-Control-Allow-Origin : This header specifies the allowed origins for cross-origin resource sharing (CORS)

## 리다이렉트

- location [[객체(Object)]]를 이용해서 리다이렉트 지정

```js
res.writeHead(302, {
	Location: '/', // 이런식으로 headers 설정에 location을 지정해서 리다이렉트를 지정할 수 있음
	'Set-Cookie': `name=${encodeURIComponent(name)}; Expires=${expires.toGMTString()}; HttpOnly; Path=/`
});
```