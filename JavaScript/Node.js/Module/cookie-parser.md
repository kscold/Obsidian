- 일반 [[http]] [[모듈(Module)]]에서 [[쿠키(Cookie)]]를 다루기 쉽지 않았다. 
- 따라서 cookie-parser [[모듈(Module)]]을 전역 [[미들웨어(Middleware)]] 등록하여 사용하면 손 쉽게 [[쿠키(Cookie)]]를 [[파싱(Parsing)]]할 수 있다.


## cookie-parser 사용

- req.cookies() [[메서드(Method)]]를 통해 [[쿠키(Cookie)]]를 설정하고 req.clearCookie [[메서드(Method)]]를 통해 쿠리를 삭제할 수 있다.
- 추가적으로 cookieParser('문자열') 형식으로 문자열을 통해 특정 [[쿠키(Cookie)]]를 암호화하고 req.signedCookies를 통해 암호화를 적용시킬 수 있다.

```js
const express = require('express');
const cookieParser = require('cookie-parser');


const app = express();

app.set('port', process.env.PORT || 8080);

app.use(cookieParser('zerochpassword'));


app.get('/', (req, res, next) => {
	req.cookies; // { mycookie: 'test' }
	req.signedCookies;
	
	res.cookie('name', encodeURIComponent(name), {
		expires: new Date(),
		httpOnly: true,
		path: '/',
	});
	
	res.clearCookie('name', encodeURIComponent(name), {
		httpOnly: true,
		path: '/',
	});
	
	res.sendFile(path.join(__dirname, 'index.html'));
});
```
