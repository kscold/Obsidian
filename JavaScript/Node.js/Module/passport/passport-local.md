- [[Express]]의 [[passport]]에서 passport-local을 사용한 구현 방법이다.


##  [[미들웨어(Middleware)]] 등록

```js
// app.js
app.use(passport.initialize());
app.use(passport.session());
```

- `app.use(passport.initialize())` passport를 사용한다고 [[Express]]에게 알린다.
- `app.use(passport.session());` session을 이용하여 passport를 동작한다.
- passport는 session을 사용하기 때문에 [[세션(Session)]]을 등록한`app.use(session({...}))` 코드 다음에 passport 미들웨어를 등록한다.


## 인증 라우터 등록

```js
app.post('/login', 
	passport.authenticate('local', { successRedirect: '/', failureRedirect: '/login' }));
```

- passport가 로그인을 처리하도록 등록하는 라우터이다.

- authenticate()는 passport [[모듈(Module)]]로 인증이 되었는지 검사하는 [[메서드(Method)]]이다.

- `/login`으로 요청이 들어왔을때 passport 실행한다.
- local 전략 사용한다.

- 인증 성공했을때 `/`으로 이동한다.
- 인증 실패했을때 `/login`으로 이한다.


## 자격검증 (Strategies)

```js
var passport = require('passport'), LocalStrategy = require('passport-local').Strategy;

passport.use(new LocalStrategy(
	{
		usernameField: 'username',
		passwordField: 'password'
	},
	
	function(username, password, done) {
		User.findOne({ username: username }, function (err, user) {
		    if (err) { 
			    return done(err); 
			}
		    if (!user) {
			    return done(null, false, { message: 'Incorrect username.' });
		    }
		    if (!user.validPassword(password)) {
			    return done(null, false, { message: 'Incorrect password.' });
			}
		return done(null, user);
    });
  }
));
```

- 요청에 대한 자격 검증을 use()를 통해 제공해줘야 한다.
- usernameFiled, passwordField에 form에서 전달해줄 id와 password의 name을 입력한다.

- passport.authenticate()에서 로그인 정보를 받으면 이를 처리하는 strategy를 미들웨어로 작성해야한다.
- username, password를 인자로 받아 검증을 하는 callback함수가 실행된다.

- 검증이 유효하면`done(null, user)` 로 user에 대한 정보를 반환한다.
- 검증에 실패하면 `done(null, false)`를 반환하고 실패한 이유를 메세지로 전달할 수 있다.

- 검사중에 [[에러(error)]]가 발생하면 `done(err)`에러를 리턴한다.


## Session

```jsx
passport.serializeUser(function(user, done) {
  done(null, user.id);
});

passport.deserializeUser(function(id, done) {
  User.findById(id, function(err, user) {
    done(err, user);
  });
});
```

- serializeUser()은 로그인에 성공했을 때 유저 정보를 session에 저장하는 기능이다.

- 로그인에서 성공했다면 strategy에서 리턴한 user가 serializeUser에 user로 전달된다.

- 로그인에 성공하면 session passport에 user값으로 user.id(사용자의 식별자)가 저장된다.
- 로그인이 됐을때 검증이 필요한 페이지에 방문할때마다 deserializeUser()가 실행된다.

- deserializseUser에서는 session에 있는 사용자의 식별자를 받아서 데이터 베이스에서 조회한다. 
- 해당 사용자를 찾아 `done(err, user)` user를 done함수에 2번째 인자로 전달하면 request.user 객체에 전달된다.


## 로그인 판별

- deserializeUser를 통해 request.user의 정보를 받을 수 있다.
- 로그인이 되었다면 request.user가 있을 것이고 로그인 되지 않았다면 request.user가 없을 것이다.


## 로그아웃

```js
app.get('/logout', function(req, res){
	req.logout();
	req.session.save((err) => {
		res.redirect('/');
	});
});
```

- req.logout()을 하면 req.user 없어진다.
- 현재의 [[세션(Session)]]상태를 session에 저장하고 리다이렉트 한다.
- req.session.destroy()를 하면 session정보를 지울 수 있다.