- [[express]]나 koa 라이브러리에서 const app = require('express') 혹은 require('Koa')로 연결을 만들어 사용한다.

- [[HTTP(Hyper Tranfer Protocol)]] [[메서드(Method)]]중에서 Get 방식을 이용하여 주소창에서 데이터를 전달하는 방식이다.
- 즉, 백엔드 API를 통해 Response를 받는데 그 과정을 만들기 위해 사용하는 [[메서드(Method)]]이다.

- 또한 [[app.set()]]으로 설정한 [[속성(Property)]]을 받을 수 있는 [[메서드(Method)]]이기도 하다.

## 문법

```js
app.get('/라우팅', (req, res) => {
	
})
```

- 이때 [[콜백 함수(Callback Function)]]의 내용 안에는 아래 코드 처럼 [[res.send()]] 함수를 사용할 수 있다.

```js
app.get('/sound/dog', (req, res) => {
	res.send('<h1>강아지</h1>') // 이 경우 처럼 HTML 문법이 실제대로도 렌더링 됨
	
	res.json({ 'sound': '멍멍' }) // json을 send 할 경우 .json()이라는 함수를 사용할 수 있음
})
```

- 또한 이때 .get()의 첫번째 [[매개변수(parameter)]]인 '라우팅' 부분에 /:key인 [[URL 파라미터]]를 사용하여 라우팅을 유동적으로 변경하였을 때 값을 저장할 수 있다.
- 이때 [[req.params]]를 사용하여 주소창의 라우팅부분을 key로 추출할 수 있다.

```js
app.get('/sound/:name', (req, res) => { // url 파라미터로
	const { name } = req.params
	
	console.log(name)
	
	if(name == "dog") { // 따라서 이런식으로 조건 걸기가 가능해짐
		res.json({ 'sound' : '멍멍' })
	}
})
```