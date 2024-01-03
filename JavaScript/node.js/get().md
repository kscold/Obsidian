app = [[express()]] 또는 app = require('Koa')

app.get('/라우팅', (req, res) => {
[[콜백 함수(callback)]]
})


[[CRUD]]의 중의 [[메서드(Method)]]인 Get 방식 -> 주소창에서 데이터를 전달하는 방식이다.
이후 백에늗 API를 통해 Rsponse를 받는데 그 과정을 만들기 위해 사용하는 백엔드 함수이다.

이 때 콜백 함수의 내용 안에는 밑의 코드 처럼 [[send()]] 함수를 사용할 수 있다.
```
app.get('/sound/dog', (req, res) => {
	res.send('<h1>강아지</h1>') // 이 경우 처럼 HTML 문법이 실제대로도 렌더링 된다.
	
	res.json({'sound': '멍멍'}) // json을 send 할 경우 .json()이라는 함수를 사용할 수 있다.
})
```

추가가적으로

또한 이때 .get()의 첫번째 [[매개변수(paramiter)]]인 '라우팅' 부분에 '/:key' 사용하여 라우팅을 유동적으로 하였을 때 값을 저장할 수 있다. 이때 [[params]]를 사용하여 주소창의 라우팅부분을 key로 추출할 수 있다.

```
app.get('/sound/:name', (req, res) => {
	const { name } = req.params
	
	console.log(name)

	if(name == "dog") { // 따라서 이런식으로 조건 걸기가 가능해짐
		res.json({'sound' : '멍멍'})
	}
}')
```