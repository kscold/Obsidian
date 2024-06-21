- [[세션(Session)]]을 관리하기 위한 [[express]] [[미들웨어(Middleware)]] [[모듈(Module)]]이다.


## seseion() 객체 설정

### secret

- 이 옵션은 어떤 키로 [[쿠키(Cookie)]]를 암호화할지 정해주는 옵션이다.
- 필수옵션이다.
### resave

- [[세션(Session)]]값의 변동이 있든 없든 항상 [[세션(Session)]]을 다시 저장해줄지 정하는 옵션이다.
- false를 추천하고 필요하면 true로 바꿀 수 있다.
- 기본값 true이다.
### saveUninitialized

- [[세션(Session)]]에 저장할 내용이 있든 없든 일단 저장할지 정하는 옵션이다.
- false로 두면 [[서버(Server)]] 용량을 줄일 수 있다.
- 기본값 true이다.
### store 

- [[세션(Session)]]을 [[서버(Server)]]에 저장할때 사용하게될 저장소를 정해준다.
- 기본값은 MemoryStore로 메모리 스토어는 [[서버(Server)]]를 재시작하거나 종료하면 저장된 [[세션(Session)]]이 없어진다.
### name

- f12 개발자 도구 탭에서 쿠키탭으로 볼 수 있는 session의 이름이다.
- 초깃값은 connect.sid이나 이름을 다른 문자열로 바꿔줄 수 있다.

## seseion() 객체 활용법

- 아래 코드 예시는 session() [[인스턴스(Instance)]] [[객체(Object)]] 설정을 통해 [[세션(Session)]]을 생성하고 [[세션(Session)]] 유무의 따른 [[미들웨어(Middleware)]]을 처리하는 방식이다.

```js
const express = require('express');

const cookieParser = require('cookie-parser');
const session = require('express-session');

  
const app = express();

  
app.set('port', process.env.PORT || 8080);

app.use(cookieParser('zerochpassword'));
app.use(
	session({
	// 세션일 때 항상 세션쿠키를 사용하기 때문
		resave: false,
		saveUninitialized: false,
		secret: 'zerochpassword',
		cookie: {
			httpOnly: true, // XSS 공격을 막기 위해 true로 설정
		},
		name: 'connect.sid',
	})
);

// 위의 미들웨어로 로그인 로직 까지 끝난 후 / 경로의 페이지를 보여주고 싶을 때 세션 유무에 따른 분기 처리
app.use('/', (req, res, next) => {
	if (req.session.id) {
		express.static({ __dirname: 'public' });
	} else {
		next();
	}
});
```