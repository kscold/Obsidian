- [[노드(Node.js)]]는 자바스크립트 [[런타임(runtime)]] 플랫폼이라고 하는데 쉽게 말해서 [[서버(Server)]]를 구축하는 데 도움이 되는 툴이다. 

- node.js는 [[HTTP(Hyper Tranfer Protocol)]]의 도구를 제공하기 때문에 http [[모듈(Module)]]을 통해서 [[서버(Server)]]에 요청하고 응답을 보내고자 한다.  

  
## http 모듈을 이용한 [[서버(Server)]] 생성

- 처음에는 다음과 같이 http 모듈을 불러오고 서버를 할당해주면 된다.
- [[http.createServer()]] [[메서드(Method)]]를 사용한다.

```javascript

const http = require('http');//모듈을 불러온다.

// 서버가 들어갈  변수를 지정
// 두 개의 인자를 사용할 수 있음

const server = http.createServer((request, response) => {
	response.writeHead(200);
	response.end('hello');
});
```

- 위에 보다시피 첫 번째 인자는 request, 두 번째 인자를 response를 받아올 수 있다.

- 그 다음으로는 포트 번호를 설정을 다음과 같이 해보았다.  
- 포트를 제공하는 서버를 호출할 수 있는지 [[listen()]]에 포트 번호를 넣어준다.

- 이 프로그램을 실행할 때 이 서버는 포트 8080에서 수신 대기하고 연결이나 새 요청이 있을 때 마다 [[서버(Server)]]는 [[이벤트(event)]]를 발생시켜 자체 [[메서드(Method)]] 또한 사용할 수 있다.  
  

```javascript
//포트번호 설정
server.listen(8080, () => {
  console.log(" server listening ")
});

```

- 명령어로 node (파일) 을 실행하면 내가 적은 콘솔이 찍히는 것을 확인할 수 있다.

```javascript
// 클라이언트에 어떤 텍스트 값을 보냄 
const server = http.createServer((req, res) => {
  res.write("hello")
  res.end() // 실행시킨 뒤 끝내기 
})

```

- 응답을 받을 때 [[res.write()]]와 [[res.end()]] 메서드를 이용하면 된다.  

- 터미널에서 서버를 열고 해당 포트 번호에 들어가서 write에 보낸 응답이 왔는지 확인 할 수 있다.

```javascript
(res.write('Hello')) // 클라이언트에 "Hello"를 보낼 수 있음
(res.end())
```

- 이제 브라우저에 포트번호를 입력하고 화면을 확인하면 "hello"가 응답을 받을 것을 확인할 수 있다.  
- 웹 또는 모바일 애플리케이션용 백엔드 서비스를 구축하려면 다양한 경로를 처리해야 하는데 예를 들면 uri , / 와 같은 경로를 조건문을 통해 쓸 수 있다.

- 밑의 코드와 같이 조건문을 사용할 때에는 기본 경로가 '/' 이므로 "localhost:3000/"로 들어가면 "hello"가 뜨고 그 밖에 "localhost:3000/(ex.fsdf)" / 뒤에 아무 문자를 입력하면 "hello Node"이런 화면을 띄울 수 있다.

```javascript
const server = http.createServer((request, response) => {
	
	if(req.url === '/') {
		res.write("hello")// 기본  '/' 경로일 때
	} else {
		res.write("hello Node")// 다른 페이지 일 때
	}
	
	res.end()
})
```

- 다음과 같이 조건문을 써서 post나 옵션을 넣을 수도 있다.

```javascript
if (req.method === 'POST'){

}

if (req.method === 'OPTIONS') {
	res.writeHead(200,{'Content-Type' : 'application/json'}); //헤더 데이터 전송할 때
    
    res.end();
}

```