- [[노드(Node.js)]]의 [[express]] 또는 koa의 미들웨어(Middleware)는 [[HTTP(Hyper Tranfer Protocol)]] 요청이 들어온 순간부터 순차적으로 시작이 된다.
- 미들웨어는 [[HTTP(Hyper Tranfer Protocol)]] 요청과 응답 [[객체(Object)]]를 처리하거나, 다음 미들웨어를 실행할 수 있다.

- [[HTTP(Hyper Tranfer Protocol)]] 응답이 마무리될 때까지 미들웨어 동작 사이클이 실행된다.

## Route Handler

- Route Handler도 미들웨어의 한 종류이다.

- 라우팅 함수(get, post, put, delete 등)에 적용된 미들웨어이다.
- 일반적인 미들웨어와는 다르게 path parameter를 사용할 수 있다.

## 미들웨어의 사용법

- req, res, next를 [[매개변수(parameter)]]로  가지는 [[콜백 함수(Callback Function)]]를 작성하면 해당 [[함수(Function)]]는 미들웨어로 동작할 수 있다.

- [[res]]는 [[HTTP(Hyper Tranfer Protocol)]] 요청을 처리하는 [[객체(Object)]](request)이다.
- [[req]]는[[HTTP(Hyper Tranfer Protocol)]] 응답을 처리하는 [[객체(Object)]](response)이다.

- [[next()]]는 다음 미들웨어를 실행하는 [[함수(Function)]]이다.
- next의 경우 인자가 있으면 그 미들웨어로 이동한다.
- 따라서 [[에러(error)]] [[객체(Object)]]을 넣어 바로 [[에러 미들웨어(Error Middleware)]]로 이동시키도록 사용한다.

```jsx
const logger = (req, res, next) => {
	console.log(`Request ${req.path}`); 
	next();
};
 
const auth = (req, res, next) => {
	if (!isAdmin(req)) {
	    next(new Error("Not Authorized"));
		return;
	}
	next();
};
```

- req, res, next를 인자로 갖는 함수를 작성하면 미들웨어가 된다.

- req, res 객체를 통해 HTTP 요청과 응답을 처리하거나 next 함수를 통해 다음 미들웨어를 호출해야 한다.

- next() 함수가 호출되지 않으면 미들웨어 사이클이 멈추기 때문에 주의해야 된다.

- 미들웨어는 적용되는 위치에 따라서 애플리케이션 미들웨어, 라우터 미들웨어, 오류처리 미들웨어로 분류가 가능하다.
- 필요한 동작 방식에 따라 미들웨어를 적용할 위치를 결정해야 된다.


## 애플리케이션 미들웨어

```js
app.use((req, res, next) => {  
	console.log(`Request ${req.path}`);  
	next(); // 1
}); 

app.use(auth); // 2 

app.get("/", (req, res, next) => { 
	res.send("Hello Express"); // 3
});
```

- use나 http method 함수를 사용하여 미들웨어를 연결할 수 있다.
- 미들웨어를 모든 요청에 공통적으로 적용하기 위한 방법이다.
- http 요청이 들어온 순간부터 **적용된 순서대로 동작한다.**

## 라우터 미들웨어

```js
router.use(auth); // 3
router.get("/", (req, res, next) => {  
	res.send("Hello Router");
}); // 4
//-----------------------------------------

app.use((req, res, next) => {  
	console.log(`Request ${req.path}`);  
	next(); // 1
}); 

app.use("/admin", router); // 2
```

- router 객체에 미들웨어가 적용되는 것 외에는 애플리케이션 미들웨어와 사용 방법은 동일하다.
- 특정 경로의 라우팅에만 미들웨어를 적용하기 위한 방법이다.
- app 객체에 라우터가 적용된 이후로 순서대로 동작한다.

## 미들웨어 서브스택

```js
app.use(middleware1, middlware2, ...); 
		
app.use("/admin", auth, adminRouter);

app.get("/", logger, (req, res, next) => {  
	res.send("Hello Express");
});
```

- 여러 개의 미들웨어를 동시에 적용할 수 있다.
- 주로 한 개의 경로에 특정해서 미들웨어를 적용하기 위해 사용한다.
- 전달된 인자의 순서 순으로 동작한다.

## 오류처리 미들웨어

- 일반적으로 가장 마지막에 위치한다.
- err, req, res, next 네 가지 인자를 가지며, 앞선 미들웨어에서 next 함수에 인자가 전달되면 실행된다.

```js
app.use((req, res, next) => {  
	if (!isAdmin(req)) {    
		next(new Error("Not Authorized")); // 1 중간은 건너뛰고 마지막 오류처리 미들웨어로 실행된다.
		return; 
	}
	next();
}); 

app.get("/", (req, res, next) => {  
	res.send("Hello Express");
}); 

app.use((err, req, res, next) => { // 2, err 인자
	res.send("Error Occurred");
});

```

- `next(new Error());`
- 가장 아래 적용된 err, req, res, next를 인자로 갖는 함수가 오류처리 미들웨어이다.
- 이전에 적용된 미들웨어 중 next에 인자를 넘기는 경우 중간 미들웨어들은 뛰어넘고 오류처리 미들웨어가 바로 실행된다.

## 함수형 미들웨어

- 하나의 미들웨어를 작성하고, **작동 모드를 선택하면서 사용**하고 싶을 경우 미들웨어를 함수형으로 작성하여 사용할 수 있다.
- Ex) API별로 사용자의 권한을 다르게 제한하고 싶은 경우. 사용자 권한을 제한하는 미들웨어를 1개만 작성하고 사용자의 권한을 미들웨어의 인자로 넘기면서 체크할 수 있도록 작성할 수 있다.

```js
const auth = (memberType) => {  
	return (req, res, next) => { // 미들웨어 함수 리턴   
		if (!checkMember(req, memberType)) {    
			next(new Error(`member not ${memberType}`));      
			return;
		}    
		next();  
	};
}; 
		
app.use("/admin", auth("admin"), adminRouter); 
app.use("/users", auth("member"), userRouter);
```

- auth 함수는 **미들웨어 함수를 반환하는 함수이다.**
- auth **함수 실행 시 미들웨어의 동작이 결정**되는 방식이다.
- 일반적으로 **동일한 로직에 설정값만 다르게** 미들웨어를 사용하고 싶을 경우에 활용된다.