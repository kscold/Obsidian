- 클라이언트가 요청하는 정적 파일인 view의 파일([[HTML(Hyper Text Markup Language)]])이 [[서버(Server)]]에서는 디렉토리 위치가 다를 수 있기 때문에 express.static() [[메서드(Method)]]를 통해 정의를 해놓을 수 있다.

- express.static()의 경우 파일을 찾으면 [[next()]]를 하지 않고 파일을 찾지 못하면 [[next()]]를 통해 다음 [[미들웨어(Middleware)]]로 동작을 넘긴다.


## 문법

```js
app.use('/', express.static(__dirnmae, 'public-3030'));
// public-3030/hello.html
```