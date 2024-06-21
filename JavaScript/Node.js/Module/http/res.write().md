- [[노드(Node.js)]]의 write()는 추가 프레임워크 [[express]] 없이,기본 [[모듈(Module)]]인 [[HTTP(Hyper Tranfer Protocol)]]만으로 가능한 기초 전송 [[메서드(Method)]]이다.

- 특징으로는 여러 번 보낼 수 있다.
- 즉, 호출할 수 있다.

- 하지만 장점이자 단점으로 head 와 전송을 끝낸다는
- [[res.end()]] [[메서드(Method)]]를 직접 지정 해줘야 한다는 점이 있다.

- [[res.writeHead()]] [[메서드(Method)]]를 통해 header 설정을 할 수 있다.

## 예시

```js
const http = require('http');

http.createServer(onRequest).listen(8080, madeServer);
// 서버를 생성

function madeServer(){
  console.log("8080 서버를 만듬!");
}

function onRequest(request, response){
	console.log("사용자가 들어옴");
	response.writeHead(200, {'Content-Type': 'text/html'});
	response.write('<h1>Hello User</h1>');
	response.end('<h1>Res Done</h1>'); // end를 안쓰면 데이터가 넘어가지 않는다!
}
```
