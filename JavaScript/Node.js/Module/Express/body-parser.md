- 현재 [[express]]에서는 bodyparser [[모듈(Module)]]을 따로 설치하지 않아도 내장이 되어 있다.
- [[express]] 4.16.0 버전이전에는 bodyParser라는 [[모듈(Module)]]을 설치해서 사용했어야 했다.

## 모듈 설치 시 사용법

```js
const bodyParser = require('body-parser');
```

## bodyparser 사용 예시

-  전역 [[미들웨어(Middleware)]]에서 express.json()과 express.urlencoded()를 설정해주면 자동적으로 데이터를 [[파싱(Parsing)]]해서 보여준다.
- 따라서 [[미들웨어(Middleware)]]로 설정을 했다면 [[req.body]]를 통해 [[객체(Object)]] 혹은 [[속성(Property)]]에 접근할 수 있다.


```js
const express = require('express');

  
const app = express();


app.set('port', process.env.PORT || 8080);

app.use(express.json());
app.use(express.urlencoded({ extended: true }));


app.get('/', (req, res, next) => {
	// req.body.객체 혹은 객체 속성을 통해서 접근가능
});
```