- 파이프(Pipe)는 [[@Injectable()]] [[데코레이터(Decorator)]]로 주석이 달린 [[클래스(class)]]이다.

- 파이프는 [[컨트롤러(Controller)]]에 제공되는 [[@Body()]], [[@Param()]]등 이미우리가 사용하고 있는 입력받는 [[데코레이터(Decorator)]]들이 모두 포함된다.
- 즉, 파이프는 인자 데이터를 가공한 후 [[컨트롤러(Controller)]] 메서드로 값들을 넘겨준다.

- 파이프는 [[Data Transformation]]과 [[Data vaildation]]을 위해서 사용된다.

- [[NestJS]]는 [[메서드(Method)]]가 호출되기 직전에 파이프를 삽입하고 파이프는 [[메서드(Method)]]로 향하는 인자를 수신하고 이에 대해 작동한다.

- 즉, 파이프는 [[Express]]의 [[미들웨어(Middleware)]]와 비슷한 개념이다.

- [[NestJS]]에서는 바로 사용할 수 있는 [[Built-in pipe]] 들을 제공해준다. 
- 해당 파이프들은 `@nestjs/common` 안에 들어있다.


## 파이프의 역할

- 아래는 [[미들웨어(Middleware)]]와 같이 작동하는 파이프의 역할을 보여주는 이미지이다.

![[Pasted image 20240901164101.png]]


## 파이프의 [[Data Transformation]]과 [[Data vaildation]]

- 라우트 핸들러(Route Handler)가 처리하는 인자에 대해서 작동한다.

- 파이프는 [[메서드(Method)]]를 바로 직전에 작동해서 [[메서드(Method)]]로 향하는 인자에 대해서 변환할 것이 있으면 변환하고 유효성 체크를 위해서도 호출된다.


## 파이프를 사용하는 방법(Binding Pipes)

- 파이프를 사용한는 방법(Binding Pipes)은 세 가지로 나룰 수 있다.
- Handler-level Pipes, Parameter-level Pipes, Global-level Pipes가 있으며 이름에서 말하는 것 그대로 [[핸들러(Handler)]] 레벨, [[매개변수(parameter)]] 레벨, 전역 레벨로 파이프를 사용할 수 있다.

### Handler-level Pipes

- [[핸들러(Handler)]] 레벨에서 [[@UsePipes()]] [[데코레이터(Decorator)]]를 이용해서 사용할 수 있다.
- 이 파이프는 모든 [[매개변수(parameter)]]에 적용된다.

```ts
@Post()
@UsePipes(pipe)
createBoard(@Body('title') title, @Body('description') description) {
	
}
```

### Parameter-level Pipes

- [[매개변수(parameter)]] 레벨의 [[파이프(Pipe)]]이기에 특정한 [[매개변수(parameter)]]에게만 적용이 되는 [[파이프(Pipe)]]이다.
- 아래와 같은 경우에는 title만 [[매개변수(parameter)]] [[파이프(Pipe)]]가 적용이 된다.

```ts
@Post()
createBoard(@Body('title', ParameterPipe) title, @Body('description') description) {
	
}
```

### Global Pipes

- 글로벌 [[파이프(Pipe)]]로서 어플리케이션 레벨의 [[파이프(Pipe)]]이다.
- 클라이언트에서 들어오는 모든 요청에 적용이 된다.
- 가장 상단 영역인 main.ts에 넣으면 된다.

```ts
async function bootstrap() {
	const app = await NestFactory.create(AppModule);
	app.useGlobalPipes(GlobalPipes);
	await app.listen(3000);
}

bootstrap();
```
  