- [[NestJS]]에서 사용할 수 있게 만들어 놓은 기본 [[파이프(Pipe)]]로 6가지의 [[파이프(Pipe)]]가 있다.


## Built-in pipe의 종류

- 이름을 보면 각각의 [[파이프(Pipe)]]가 어떤 역할을 하는지 짐작할 수 있다.

### [[ValidationPipe]]

- validation pipe은 [[class-validator]] 패키지와 해당 패키지에서 제공하는 데코레이터를 활용하여 요청 데이터의 유효성을 검증하는 기능이다.

### [[ParseIntPipe]]

- 원래 [[매개변수(parameter)]] 값으로 숫자가 들어와야하는 [[핸들러(Handler)]]가 있다고 가정한다.

```ts
@Get(':id')
findOne(@Param('id', ParseIntPipe) id: number) {
	return ;
}
```

- 하지만 [[매개변수(parameter)]] 값으로 숫자가 아닌 abc와 같은 문자열을 GET http://localhost:8080/boards/abc 로 보내개 된다면 400(BadRequest) 오류가 난다.

### ParseBoolPipe

### ParseArrayPipe

### ParseUUIDPipe

### [[DefaultValuePipe]]


