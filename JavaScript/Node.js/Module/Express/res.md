- [[노드(Node.js)]]에서 [[express]]에서 사용되는 응답 [[객체(Object)]]이다.

 - 주의해야될 점으로 [[res.end()]], res.json(), res.redirect(), res.render(), [[res.send()]], [[res.sendFile()]]는 각 라우터에 반드시 한번만 써야 한다.


## res의 [[메서드(Method)]]

### res.app

- 똑같이 res [[객체(Object)]]를 통해 app [[객체(Object)]]에 접근한다. 
- res.[[app.get()]]('')같이 사용 가능하다.
### res.set(헤더, 값)/res.setHeader(헤더, 값)

- 응답의 헤더를 설정한다.
- req.get()이 헤더값을 가져오는거라면 이건 헤더를 설정하는 것이다.
### res.status(코드) / res.sendStatus(코드)

- 응답 시의 HTTP 상태 코드를 지정한다.
### res.type(type)

- Contents-Type 헤더를 설정할 수 있는 간단한 메서드이다.
### res.cookie(키, 값, 옵션)

- [[쿠키(Cookie)]]를 응답에 설정하는 메서드이다.
- cookie-parser 패키지가 필요하다.
### res.clearCookie(키, 값, 옵션)

- [[쿠키(Cookie)]]를 응답에서 제거하는 [[메서드(Method)]]이다.  
### [[res.end()]]

- 데이터 없이 응답을 보낸다.
### res.json(JSON)

- [[JSON(Java Script Object Notation)]] 형식의 응답을 보낸다.
### res.redirect(주소)

- 리다이렉트할 주소와 함께 응답을 보낸다.
### res.locals / res.render(뷰, 데이터)

- res.locals는 뷰를 렌더링하는 기본 콘텍스트를 포함하는 객체다. 
- res.render는 jade와 같은 템플릿 엔진을 사용하여 뷰를 렌더링한다.
### [[res.send()]], [[매개변수(parameter)]](status, body)

- 클라이언트에 응답을 보낸다.
- 상태 코드는 옵션이다.
- 기본 콘텐츠 타입은 text/html이므로 text/plain을 보내려면 res.set(‘Content-Type’, ‘text/plain’)을 먼저 호출 해야한다.
### [[res.sendFile()]], [[매개변수(parameter)]](경로)

- 경로에 위치한 파일을 응답한다.  
### res.attachment(파일 이름), res.download() [[매개변수(parameter)]]([[path]], 파일이름, [[콜백 함수(Callback Function)]])

- 클라이언트에게 파일을 표시하지 말고 다운로드 받으라고 전송한다.
- filename을 주면 파일 이름이 명시되며, res.attachment는 헤더만 설정하므로 다운로드를 위한 node 코드가 따로 필요하다.

## res [[객체(Object)]] [[메서드(Method)]] 사용 예시
### res.json(JSON)

```js
// 이 부분을 하나로 짧게 합친 express 메서드
res.writeHead(200, { 'Content-Type': 'application/json' });
res.end(JSON.stringfy({hello: 'nomad'})); // json 형식으로 만듬

// 

res.json({hello: 'nomad'})​​
```
### [[res.send()]]

```js
res.send(Buffer.from('whoop'))
res.send({ some: 'json' }) // res.status(200).send({ some: 'json' }) 와 같음, 200이 생략되어있음
res.send('<p>some html</p>')

res.status(404).send('Sorry, we cannot find that!')
res.status(500).send({ error: 'something blew up' })
```
### res.status()

```js
// http모듈에서 두줄이 코드가 하나로 줄어들었다고 보면 된다.
// res.writeHead(200, { 'Content-Type', 'text/html'});
// res.end("안녕하세요");

res.status(403).end()
res.status(400).send('Bad Request')
res.status(404).sendFile('/absolute/path/to/404.png')
```
### res.sendStatus()

```
res.sendStatus(200) // == res.status(200).send('OK')
res.sendStatus(403) // == res.status(403).send('Forbidden')
res.sendStatus(404) // == res.status(404).send('Not Found')
res.sendStatus(500) // == res.status(500).send('Internal Server Error')
```
### res.redirect()

```js
// 기존 http모듈 코드
res.writeHead(301, {
	Location: 'http://example.com',
    // 'Set-Cookie': '~',
});
res.end();

// 익스프레스 코드
res.redirect(301, 'http://example.com') // 301로 해당페이지로 강제이동
res.redirect('/foo/bar')
res.redirect('http://example.com')
res.redirect('../login')
```
### res.render()

```js
// send the rendered view to the client
res.render('index')

// if a callback is specified, the rendered HTML string has to be sent explicitly
res.render('index', function (err, html) {
  res.send(html)
})

// pass a local variable to the view
res.render('user', { name: 'Tobi' }, function (err, html) {
  // ...
})
```
