- 클라이언트에서 서버로 request를 보내고 내용을 읽고 처리한 후 response를 보낸다.

- 요청과 응답은 이벤트 방식이다. 
- 요청이 왔을 때 어떤 작업을 수행할지 [[이벤트(event)]] 리스너를 미리 등록해두어야 한다.
- [[웹(web)]] 브라우저에서 요청을 처리하기 위해서 [[HTTP(Hyper Tranfer Protocol)]] 서버가 있어야 한다.

- 서버의 종류에는 뷰(view)로 서비스가 되는 [[웹 서버(Web Server)]]가 있고 [[JSON(Java Script Object Notation)]]형식으로 제공되는 api 서버가 있다.


## 요청과 응답

- [[HTTP(Hyper Tranfer Protocol)]] [[모듈(Module)]]의 [[http.createServer()]] [[메서드(Method)]]를 사용해보자.

```js
const http = require('http');

http.createServer((req, res) => {
	res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' });
	res.write('<h1>Hello Node!</h1>');
	res.end('<p>Hello Server!</p>');
}).listen(8080, () => { // 서버 연결
	console.log('8080번 포트에서 서버 대기 중입니다!');
});
```

- 위 코드를 실행시키고 http://localhost:8080 또는 http://127.0.0.1:8080 에 접속하면 아래 같은 화면이 나온다.

![](https://blog.kakaocdn.net/dn/s3x64/btrqzRwApOg/0a6NlkHYFE9TOuwkwuNxFk/img.png)

- localhost는 현재 컴퓨터의 내부 주소이다. 127.0.0.1을 대신 사용해도 된다.
- 포트는 서버 내에서 프로세스를 구분하는 번호이다.
- 즉 서버는 프로세스에 포트를 다르게 할당하여 들어오는 요청을 구분한다.

- http(80), 3306(mysql) 등으로 붙인다.

![](https://blog.kakaocdn.net/dn/GAkyk/btrqzSvwesz/OGc7DhFDh5EfcvckaheCRk/img.png)

- 즉 위의 예시는 8080포트 번호에 노드 서버(프로세스)를 연결하였다.
- 실제로 배포할때는 80또는 443을 사용할 것이다.

- 리눅스와 맥에서는 1024번 이하의 포트에 연결할 때 관리자 권한이 필요하다. (sudo node server)

- [[http.createServer()]] 뒤에 [[listen()]] [[메서드(Method)]]를 붙인다. 
- 인자로 공개할 포트번호와 [[콜백 함수(Callback Function)]]을 넣어준다.

- 즉 8080포트로 요청을 기다리고 있는 상태이다.
- res.writeHead에서 200은 성공적인 요청임을 의미한다. 
- 두번째 인수는 응답에 대한 정보를 보내는데 이 정보들이 기록되는 부분을 헤더라고 한다.


- res.write의 첫번째 인수는 클라이언트에게 보낼 데이터이다.
- 이 부분이 body이다.

- res.end는 인수로 받은 값이 있다면 데이터를 클라이언트에게 보내고 응답을 종료한다.

![](https://blog.kakaocdn.net/dn/NXJAk/btrqAbVWPiW/fKIpf8huJqqdKWmQMzRwy1/img.png)

```js
const http = require('http');

const server = http.createServer((req, res) => {
	res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' });
	// 헤더 설정을 통해 html 태그를 정확히 인식시킬 수 있고 한글도 인식가능하게 만듬
	res.write('<h1>Hello Node!</h1>');
	res.end('<p>Hello Server!</p>');
});
server.listen(8080);

server.on('listening', () => {
	console.log('8080번 포트에서 서버 대기 중입니다!');
});
server.on('error', (error) => {
	console.error(error);
});
```

- [[listen()]] [[메서드(Method)]]에 [[콜백 함수(Callback Function)]]을 넣지 않고 listening, error등의 [[이벤트 리스너(Event Listener)]]를 붙여도 된다.

```js
const http = require('http');

http.createServer((req, res) => {
	res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' });
	res.write('<h1>Hello Node!</h1>');
	res.end('<p>Hello Server!</p>');
}).listen(8080, () => { // 서버 연결
	console.log('8080번 포트에서 서버 대기 중입니다!');
});

http.createServer((req, res) => {
	res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' });
	res.write('<h1>Hello Node!</h1>');
	res.end('<p>Hello Server!</p>');
}).listen(8081, () => { // 서버 연결
	console.log('8081번 포트에서 서버 대기 중입니다!');
});
```

- 포트번호를 다르게하여 한번에 여러 서버를 실행할 수 있다.

```html
// server2.html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>Node.js 웹 서버</title>
</head>
<body>
    <h1>Node.js 웹 서버</h1>
    <p>만들 준비되셨나요?</p>
</body>
</html>
```

```js
// server2.js
const http = require('http');
const fs = require('fs').promises;

http.createServer(async (req, res) => {
	try {
	    const data = await fs.readFile('./server2.html');
	    res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' });
	    res.end(data);
	} catch (err) {
	    console.error(err);
	    res.writeHead(500, { 'Content-Type': 'text/plain; charset=utf-8' });
	    res.end(err.message);
	}
}).listen(8081, () => {
	console.log('8081번 포트에서 서버 대기 중입니다!');
});
```

- 위 예시는 8081번으로 클라이언트의 요청이 들어왔을 때, html 파일을 fs로 읽어와서 응답으로 전송한다.

## REST와 [[라우팅(Routing)]] 사용하기

- 서버에 요청을 보낼 때는 주소를 통해 요청의 내용을 표현한다. 
- 주소가  /index.html이면 서버의 index.html을 보내달라는 것이다.

- html말고 css, js 혹은 이미지 파일을 요청할수도 있다. 
- 이런 요청의 내용은 주소를 통해 표현된다.

- REST는 (REpresentational State Transfer) 서버의 자원을 정의하고 자원에 대한 주소를 지정하는 방법이다.

- 즉 서버에 요청을 보낼 때 사용하는 주소 체계를 말한다.

- 예를들면 /user이면 사용자와 관련된 자원을 요청하는 것이고 /post라면 게시글에 관련된 자원을 요청하는 것이라고 추측할 수 있다.

- 여기에 http 요청 메서드를 사용해서 좀 더 명확하게 동작을 행하게 할 수 있다.

## HTTP 메서드

### GET

- 서버 자원을 가져올 때, 요청의 본문에 데이터를 넣지 않는다.
- 데이터를 서버로 보내야 한다면 [[쿼리스트링(Querystring)]]을 사용한다.
### POST

- 서버에 자원을 새로 등록하고자 할 때. 요청의 본문에 새로 등록할 데이터를 넣어 보낸다.
### PUT

- 서버의 자원을 요청에 들어 있는 자원으로 치환하고자 할 때. 요청의 본문에 치환할 데이터를 넣어 보낸다.
### PATCH

- 서버 자원의 일부만 수정하고자 할 때. 요청의 본문에 일부 수정할 데이터를 넣어 보낸다.
### DELETE

- 서버의 자원을 삭제하고자 할 때. 요청의 본문에 데이터를 넣지 않는다.
### OPTIONS

- 요청을 하기 전에 통신 옵션을 설명하기 위해 사용한다.

![](https://blog.kakaocdn.net/dn/bZwmU3/btrqzRwKjLQ/6zikLlCD9AjHc0j9SYq4AK/img.png)

- 예) GET 메서드의 /user 주소로 요청을 보내면 사용자 정보를 가져오는 요청이다.

- 예) POST 메서드의 /user 주소로 요청을 보내면 새로운 사용자를 등록한다.

- 이렇게 배운 REST 주소 체계를 따르는 서버를 RESTful하다고 한다.

- 실습에 앞서 먼저 대략적인 주소를 먼저 설계한다.

![](https://blog.kakaocdn.net/dn/ctlvX1/btrquioPqxz/LNWl3Fd6ySwVWl5YW4Bjyk/img.png)

- 이 책의 저자의 깃허브 저장소의 코드를 가지고 실습한다.

```js
const http = require('http');
const fs = require('fs').promises;

const users = {}; // 데이터 저장용

http.createServer(async (req, res) => {
  try {
    if (req.method === 'GET') {
      if (req.url === '/') {
        const data = await fs.readFile('./restFront.html');
        res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' });
        return res.end(data);
      } else if (req.url === '/about') {
        const data = await fs.readFile('./about.html');
        res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' });
        return res.end(data);
      } else if (req.url === '/users') {
        res.writeHead(200, { 'Content-Type': 'application/json; charset=utf-8' });
        return res.end(JSON.stringify(users));
      }
      // /도 /about도 /users도 아니면
      try {
        const data = await fs.readFile(`.${req.url}`);
        return res.end(data);
      } catch (err) {
        // 주소에 해당하는 라우트를 못 찾았다는 404 Not Found error 발생
      }
    } else if (req.method === 'POST') {
      if (req.url === '/user') {
        let body = '';
        // 요청의 body를 stream 형식으로 받음
        req.on('data', (data) => {
          body += data;
        });
        // 요청의 body를 다 받은 후 실행됨
        return req.on('end', () => {
          console.log('POST 본문(Body):', body);
          const { name } = JSON.parse(body);
          const id = Date.now();
          users[id] = name;
          res.writeHead(201, { 'Content-Type': 'text/plain; charset=utf-8' });
          res.end('ok');
        });
      }
    } else if (req.method === 'PUT') {
      if (req.url.startsWith('/user/')) {
        const key = req.url.split('/')[2];
        let body = '';
        req.on('data', (data) => {
          body += data;
        });
        return req.on('end', () => {
          console.log('PUT 본문(Body):', body);
          users[key] = JSON.parse(body).name;
          res.writeHead(200, { 'Content-Type': 'text/plain; charset=utf-8' });
          return res.end('ok');
        });
      }
    } else if (req.method === 'DELETE') {
      if (req.url.startsWith('/user/')) {
        const key = req.url.split('/')[2];
        delete users[key];
        res.writeHead(200, { 'Content-Type': 'text/plain; charset=utf-8' });
        return res.end('ok');
      }
    }
    res.writeHead(404);
    return res.end('NOT FOUND');
  } catch (err) {
    console.error(err);
    res.writeHead(500, { 'Content-Type': 'text/plain; charset=utf-8' });
    res.end(err.message);
  }
})
  .listen(8082, () => {
    console.log('8082번 포트에서 서버 대기 중입니다');
  });
```

- req.method로 요청 메서드를 구분한다.

- 요청 메서드 종류를 구분하고나면 req.url로 요청 주소를 구분하여 해당하는 소스를 보내준다.

- req.on('data'), req.on('end')처럼 사용하는 이유는 req, res도 내부적으로 스트림으로 되어있기 때문이다. 즉 데이터를 꺼내거나 수정할때 데이터가 스트림 형식으로 전달되기 때문에 이벤트처리를 해야 된다.

![](https://blog.kakaocdn.net/dn/0AXtP/btrqAUhbfLR/OjKXA8lg8cF5XngEwyIi00/img.png)

- name 주소로 요청을 보낸다. 

- 메서드는 요청의 종류를 나타내고 xhr은 AJAX 요청이라는 뜻이다.

![](https://blog.kakaocdn.net/dn/baHGM5/btrqAnRlvpf/a3KSq21TWfBtS5S0qPFrKK/img.png)

- 이렇게 요청에 대한 응답 헤더, 본문을 살펴볼수 있다. 

- 헤더는 요청/응답에 대한 정보를 가지고 있고 본문은 실제 서버와 클라이언트가 주고받는 데이터를 담아두는 공간이다.