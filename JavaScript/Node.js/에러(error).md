- [[노드(Node.js)]]에서 발생하는 에러를 처리하는 방법에는 여러가지가 있다.


## try/catch문으로 예외를 처리

- 에러가 발생할 만한 곳을 try/catch로 감싼다.

```js
setInterval(() => {
	console.log('시작');
	try {
		throw new Error('서버를 고장내주마!');
	} catch (err) {
		console.error(err);
	}
}, 1000);
```

- [[express]]와 같은 [[모듈(Module)]]에서는 err를 [[매개변수(parameter)]] next에 넣어준다.

```js
app.use((req, res, next) => {
	console.log('요청을 실행')
	next()
}, (req, res, next) => {
	try {
		console.log("에러")
	} catch(err){
		next(err); // next로 err 객체로 이동되도록 만듬
	}
})
```


## [[콜백 함수(Callback Function)]]의 첫번째 [[매개변수(parameter)]]로 예외를 처리

- [[콜백 함수(Callback Function)]]의 첫번째 [[매개변수(parameter)]]인 err를 통해 에러를 처리한다.

```js
const fs = require('fs');

setInterval(() => {
	fs.unlink('./abcdefg.js', (err) => {
		if (err) {
			console.error(err);
		}
	});
}, 1000);
```


## 최후의 수단으로 [[process]] [[모듈(Module)]]에 [[이벤트(event)]] 등록

- [[콜백 함수(Callback Function)]]의 동작이 보장되지 않아 복구 작업용으로 쓰는 것은 부적합하지만 에러 내용 기록용으로는 쓸 수 있는 방법이다.

```js
process.on('uncaughtException', (err) => {
	console.log('예기치 못한 에러', err);
})

setInterval(() => {
	throw new Error('서버를 고장내주마!');
}, 1000);

setTimeout(() => {
	console.log('실행됩니다');
}, 2000);
```