- [[Node.js/Nest.js/NestJS]]에서는 [[Express]]에서 사용하는 [[req]].body를 상용하는 대신 @Body [[데코레이터(Decorator)]]를 사용해서 클라이언트에서 요청하는 데이터를 받을 수 있다.


## 문법

- 아래 코드는 body 값을 받기위해서 작성된 부분이며 바디값으로 posting 라는 값을 받는다.
- Posting의 타입은 미리 만들어둔 dto파일에서 import한 Posting 의 값으로 설정하겠다는 부분이다.

```ts
@Post()
createBoard(@Body() posting:Posting) {
	console.log('posting', posting);
}
```

- 위처럼 @Body()라고 사용하면 request 객체의 모든 [[속성(Property)]]을 받을 수 있고, 아래 코드처럼 @Body() [[데코레이터(Decorator)]]의 특정 속성의 문자열을 [[매개변수(parameter)]]로 사용하면 특정 [[속성(Property)]]만 받을 수 있다.

```ts
@Post()
createBoard(@Body('title') title: string, @Body('description') description: string) {
	console.log('title', title);
	console.log('description', description);
}
```