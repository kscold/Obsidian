- [[NestJS]]에서 예외 필터는 애플리케이션에서 발생하는 모든 예외를 캐치하고 처리하는 메커니즘이다.
- 예외 필터는 NestJS의 [[예외 레이어(Exception Layer)]]의 핵심 구성 요소로, 예외를 일관된 형식의 응답으로 변환한다.
- 기본적으로 NestJS는 처리되지 않은 모든 예외를 자동으로 처리하는 전역 예외 필터를 제공한다.
- 개발자는 필요에 따라 커스텀 예외 필터를 구현하여 예외 처리 로직을 확장할 수 있다.


- [[NestJS]]는 내장된 예외들은 @nestjs/common 패키지에서 HttpException 클래스를 기본으로 제공한다.
- BadRequestException, UnauthorizedException, NotFoundException 등의 다양한 HTTP 예외 클래스가 있다.
- 모든 내장 예외는 HttpException을 상속받는다.

## 예외 계층 구조

- [[NestJS]]의 예외 처리는 계층적으로 동작한다.
- [[메서드(Method)]] 레벨의 필터가 가장 먼저 적용된다.
- [[컨트롤러(Controller)]] 레벨의 필터가 그 다음에 적용된다.
- 글로벌 레벨의 필터가 마지막으로 적용된다.

- 상위 레벨에서 처리되지 않은 예외는 하위 레벨로 전파된다.


## NestJS의 예외 처리 메커니즘

- NestJS는 자체적인 [[예외 레이어(Exception Layer)]]를 통해 예외를 처리한다.
- 애플리케이션에서 예외가 발생하면 NestJS의 예외 레이어에서 이를 캐치한다.
- 처리되지 않은 예외는 기본 예외 필터에 의해 자동으로 처리된다.
- 기본 예외 필터는 예외를 사용자 친화적인 JSON 응답으로 변환한다.

```json
{
  "statusCode": 500,
  "message": "Internal server error"
}
```

## 내장 예외 필터의 작동 방식

- 모든 예외가 처리되지 않으면 최종적으로 내장 전역 예외 필터에 도달한다.
- 내장 예외 필터는 예외의 유형에 따라 적절한 HTTP 상태 코드를 결정한다:
  - `HttpException` 또는 그 파생 클래스: 예외에 정의된 상태 코드 사용
  - 인식되지 않은 예외: 기본적으로 500 (Internal Server Error) 상태 코드 사용
- 응답 본문은 예외 메시지와 상태 코드를 포함한 JSON 객체로 구성된다.

## 예외 트랜스포메이션

- NestJS는 인식되지 않은 예외를 `InternalServerErrorException`으로 자동 변환한다.
- 이 과정을 '예외 트랜스포메이션'이라고 한다.
- 덕분에 모든 예외가 일관된 형식으로 클라이언트에 반환된다.

```typescript
// 내부적으로 다음과 같이 변환됨
throw new InternalServerErrorException({
  message: exception.message,
  cause: exception
});
```

## 더 다양한 내장 HTTP 예외 클래스

NestJS는 다양한 HTTP 상태 코드에 맞는 예외 클래스를 제공한다:

- `BadRequestException` (400)
- `UnauthorizedException` (401)
- `ForbiddenException` (403)
- `NotFoundException` (404)
- `MethodNotAllowedException` (405)
- `NotAcceptableException` (406)
- `RequestTimeoutException` (408)
- `ConflictException` (409)
- `GoneException` (410)
- `PayloadTooLargeException` (413)
- `UnsupportedMediaTypeException` (415)
- `UnprocessableEntityException` (422)
- `InternalServerErrorException` (500)
- `NotImplementedException` (501)
- `BadGatewayException` (502)
- `ServiceUnavailableException` (503)
- `GatewayTimeoutException` (504)

## 커스텀 예외 필터 구현의 상세 예제

### 로깅 기능이 포함된 예외 필터

```typescript
@Catch()
export class AllExceptionsFilter implements ExceptionFilter {
  constructor(private readonly logger: Logger) {}

  catch(exception: unknown, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse();
    const request = ctx.getRequest();

    const status = 
      exception instanceof HttpException
        ? exception.getStatus()
        : HttpStatus.INTERNAL_SERVER_ERROR;

    const message = 
      exception instanceof HttpException
        ? exception.message
        : '내부 서버 오류';

    const errorResponse = {
      statusCode: status,
      timestamp: new Date().toISOString(),
      path: request.url,
      method: request.method,
      message: message
    };

    this.logger.error(
      `${request.method} ${request.url}`,
      exception instanceof Error ? exception.stack : '',
      'ExceptionFilter'
    );

    response
      .status(status)
      .json(errorResponse);
  }
}
```

### 데이터베이스 예외를 처리하는 필터

```typescript
@Catch(TypeORMError)
export class DatabaseExceptionFilter implements ExceptionFilter {
  catch(exception: TypeORMError, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse();
    
    let status = HttpStatus.INTERNAL_SERVER_ERROR;
    let message = '데이터베이스 오류가 발생했습니다.';
    
    // 특정 데이터베이스 예외 처리
    if (exception.message.includes('duplicate key')) {
      status = HttpStatus.CONFLICT;
      message = '이미 존재하는 데이터입니다.';
    } else if (exception.message.includes('not found')) {
      status = HttpStatus.NOT_FOUND;
      message = '데이터를 찾을 수 없습니다.';
    }
    
    response.status(status).json({
      statusCode: status,
      message: message,
      timestamp: new Date().toISOString()
    });
  }
}
```

## 예외 필터의 적용 범위와 우선순위

NestJS는 다양한 수준에서 예외 필터를 적용할 수 있다:

### 1. 메서드 수준 (가장 높은 우선순위)

```typescript
@Get()
@UseFilters(HttpExceptionFilter)
findAll() {
  throw new BadRequestException();
}
```

### 2. 컨트롤러 수준

```typescript
@Controller('cats')
@UseFilters(HttpExceptionFilter)
export class CatsController {}
```

### 3. 전역 수준 (가장 낮은 우선순위)

```typescript
// main.ts
app.useGlobalFilters(new HttpExceptionFilter());

// 또는 의존성 주입을 사용하는 방식
// app.module.ts
@Module({
  providers: [
    {
      provide: APP_FILTER,
      useClass: HttpExceptionFilter,
    },
  ],
})
export class AppModule {}
```

- 같은 예외에 대해 여러 필터가 적용될 경우, 가장 구체적인(메서드 수준) 필터가 우선 적용된다.
- 전역 필터는 메서드나 컨트롤러 수준의 필터가 없는 경우에만 적용된다.

## 의존성 주입을 활용한 예외 필터

예외 필터에서 다른 서비스나 프로바이더를 사용할 수 있다:

```typescript
@Injectable()
export class HttpExceptionFilter implements ExceptionFilter {
  constructor(
    private readonly configService: ConfigService,
    private readonly loggerService: LoggerService
  ) {}

  catch(exception: HttpException, host: ArgumentsHost) {
    // 구성 서비스와 로거 서비스를 사용한 예외 처리
    const isDevelopment = this.configService.get('NODE_ENV') === 'development';
    
    // 개발 환경에서만 자세한 오류 정보 포함
    if (isDevelopment) {
      this.loggerService.error(exception.stack);
    }
    
    // 나머지 예외 처리 로직
  }
}
```

## 예외 처리 모범 사례

1. **일관된 응답 형식 유지**
   - 모든 예외 응답이 동일한 형식을 갖도록 표준화한다.
   - 클라이언트가 쉽게 처리할 수 있는 일관된 구조를 제공한다.

2. **세부 정보 적절히 노출**
   - 프로덕션 환경에서는 민감한 오류 세부 정보를 노출하지 않는다.
   - 개발 환경에서만 상세한 오류 정보를 제공한다.

3. **비즈니스 예외와 시스템 예외 구분**
   - 비즈니스 규칙 위반(예: 유효성 검사 실패)과 시스템 오류(예: 데이터베이스 연결 실패)를 명확히 구분한다.

4. **로깅과 모니터링 통합**
   - 예외 필터를 로깅 시스템과 통합하여 모든 예외를 기록한다.
   - 중요한 예외에 대한 알림 시스템을 구축한다.

## 결론

- NestJS의 예외 필터는 애플리케이션의 오류 처리를 일관되고 구조적으로 관리할 수 있게 해준다.
- 내장 예외 계층과 커스텀 예외 필터를 조합하여 견고한 오류 처리 시스템을 구축할 수 있다.
- 다양한 적용 범위와 우선순위를 통해 유연한 예외 처리가 가능하다.
- 예외 필터를 통해 비즈니스 로직과 오류 처리 로직을 효과적으로 분리할 수 있다.

예외 필터를 효과적으로 활용하면 애플리케이션의 안정성을 높이고 사용자 경험을 개선할 수 있다.
