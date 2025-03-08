- 필터는 [[NestJS 모듈(module)]]에서 가장 마지막에 동작하는 필터는 [[NestJS]]에서 예외 처리를 담당하는 [[컴포넌트(Component)]]이다.
- 예외(exception)가 발생했을 때 클라이언트에게 적절한 응답을 보내는 역할을 한다.

- [[NestJS]]의 [[실행 컨텍스트(execution context)]] 파이프라인에서 가장 마지막에 동작하는 요소이다.

- [[HttpException]] [[클래스(class)]]를 [[상속(Inheritance)]]받아 사용자 정의 예외를 만들 수 있다.
- 필터는 ExceptionFilter 인터페이스([[Interface]])를 구현해야 한다.


## 필터의 동작 과정

- 아래 이미지는 가장 마지막에 동작하는 필터의 동작 방식으로 라우트(Route), [[컨트롤러(Controller)]], 전역(Global) 형식으로 동작한다.

![[Pasted image 20250303201617.png]]


## 사용 예시

### 컨트롤러 레벨 적용

```ts
@Controller('cats')
@UseFilters(HttpExceptionFilter)
export class CatsController {
	@Get()
	findAll() {
		throw new HttpException('금지됨', HttpStatus.FORBIDDEN);
	}
}
```

### 글로벌 레벨 적용

```ts
async function bootstrap() {
	const app = await NestFactory.create(AppModule);
	app.useGlobalFilters(new HttpExceptionFilter());
	
	await app.listen(3000);
}

bootstrap();
```

### 메서드 레벨 적용

```ts
@Controller('cats')

export class CatsController {
	@Get()
	@UseFilters(HttpExceptionFilter)
	findAll() {
		throw new HttpException('금지됨', HttpStatus.FORBIDDEN);
	}
}
```


## 예시

```ts

// custom-exception.filter.ts

@Catch(HttpException)

export class CustomExceptionFilter implements ExceptionFilter {

  catch(exception: HttpException, host: ArgumentsHost) {

    const ctx = host.switchToHttp();

    const response = ctx.getResponse<Response>();

    const request = ctx.getRequest<Request>();

    const status = exception.getStatus();

    response.status(status).json({

      statusCode: status,

      timestamp: new Date().toISOString(),

      path: request.url,

      message: exception.message,

      success: false,

      data: null

    });

  }

}
```

```ts
// app.module.ts
@Module({
	providers: [
		{
			provide: APP_FILTER,
			useClass: CustomExceptionFilter,
		},
	],
})

export class AppModule {}
```

- 필터를 사용하면 애플리케이션의 예외 처리를 일관되게 관리할 수 있다.
- [[HTTP 응답 상태(status)]]를 적절하게 설정하여 클라이언트에게 오류 상황을 명확하게 전달할 수 있다.

- 모든 예외를 하나의 형식으로 일관되게 처리할 수 있다.
- 예외가 발생한 경우에도 일관된 응답 형식을 유지하여 클라이언트 개발을 용이하게 한다.
- 로깅, 모니터링 등의 부가 기능을 예외 처리 과정에 통합할 수 있다.
- 특정 예외 유형에 따라 다른 처리 로직을 적용할 수 있다.
- @Catch() [[데코레이터(Decorator)]]에 여러 예외 타입을 지정하여 다양한 예외를 하나의 필터로 처리할 수 있다.

