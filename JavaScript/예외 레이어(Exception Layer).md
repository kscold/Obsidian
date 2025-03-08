- [[NestJS]]의 예외 레이어는 애플리케이션에서 발생하는 모든 예외를 처리하는 아키텍처 구성 요소이다.
- 이 레이어는 요청 처리 과정에서 발생한 예외를 일관된 방식으로 관리하고 클라이언트에게 적절한 응답을 반환한다.
- [[NestJS]]의 예외 레이어는 요청-응답 사이클의 마지막 단계에서 작동한다.
- 처리되지 않은 모든 예외는 이 레이어에 의해 캡처되고 변환된다.


## 예외 레이어의 구조

- 예외 레이어는 크게 다음 구성 요소로 이루어진다.

  1. 예외 클래스 계층(Exception Hierarchy)
	  - 다양한 유형의 예외를 정의하는 [[클래스(class)]] 구조이다.
  2. [[예외 필터(Exception Filter)]]
	  - 예외를 캐치하고 처리하는 [[필터(Filter)]] 메커니즘이다.
  3. 예외 트랜스포메이션(Exception Transformation)
	  - 예외를 적절한 [[HTTP 응답 상태(status)]] 응답으로 변환하는 프로세스이다.

## 예외 처리 흐름

1. **예외 발생**: 컨트롤러, 서비스, 가드, 파이프 등에서 예외가 발생한다.
2. **예외 필터 적용**: 적용된 예외 필터가 순서대로 실행된다(메서드 → 컨트롤러 → 글로벌).
3. **필터 없을 경우 기본 처리**: 설정된 필터가 없으면 기본 글로벌 예외 필터가 처리한다.
4. **예외 트랜스포메이션**: 인식되지 않은 예외는 `InternalServerErrorException`으로 자동 변환된다.
5. **응답 생성**: 최종적으로 HTTP 응답이 생성되어 클라이언트에 반환된다.

## 기본 예외 레이어 메커니즘

### 자동 예외 변환

- NestJS는 처리되지 않은 모든 예외를 자동으로 적절한 HTTP 응답으로 변환한다.
- `HttpException` 타입의 예외는 정의된 상태 코드와 메시지를 사용한다.
- 다른 모든 예외는 500 Internal Server Error로 변환된다.

```typescript
// 컨트롤러에서의 예외 발생
@Get()
findAll() {
  throw new HttpException('금지됨', HttpStatus.FORBIDDEN);
  // 클라이언트는 403 상태 코드와 메시지를 받는다
}

@Get(':id')
findOne(@Param('id') id: string) {
  throw new Error('데이터베이스 연결 실패');
  // 자동으로 500 상태 코드로 변환된다
}
```

### 내장 글로벌 예외 필터

- NestJS는 내장된 글로벌 예외 필터를 제공하여 처리되지 않은 모든 예외를 캡처한다.
- 이 필터는 예외를 JSON 형식의 응답으로 변환한다:

```json
{
  "statusCode": 403,
  "message": "금지됨"
}
```

## 예외 레이어 확장

### 사용자 정의 예외 생성

- `HttpException`을 상속하여 도메인별 예외를 정의할 수 있다:

```typescript
export class ProductNotFoundException extends HttpException {
  constructor(productId: string) {
    super(`상품 ID ${productId}를 찾을 수 없습니다`, HttpStatus.NOT_FOUND);
  }
}
```

### 예외 필터 확장

- `ExceptionFilter` 인터페이스를 구현하여 예외 처리 방식을 확장할 수 있다:

```typescript
@Catch(HttpException)
export class HttpExceptionFilter implements ExceptionFilter {
  catch(exception: HttpException, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse();
    const request = ctx.getRequest();
    const status = exception.getStatus();

    response.status(status).json({
      statusCode: status,
      timestamp: new Date().toISOString(),
      path: request.url,
      error: exception.name,
      message: exception.message
    });
  }
}
```

## 예외 레이어의 고급 기능

### 예외 계층별 처리

- 다양한 유형의 예외를 선택적으로 처리할 수 있다:

```typescript
@Catch(TypeError, ValidationError)
export class TypeErrorFilter implements ExceptionFilter {
  catch(exception: TypeError | ValidationError, host: ArgumentsHost) {
    // 특정 유형의 예외 처리
  }
}
```


### 예외 마스킹

- 프로덕션 환경에서는 민감한 오류 정보를 숨기고 일반화된 메시지를 사용할 수 있다:

```typescript
@Injectable()
export class ProductionExceptionFilter implements ExceptionFilter {
  constructor(private readonly configService: ConfigService) {}

  catch(exception: unknown, host: ArgumentsHost) {
    const isProduction = this.configService.get('NODE_ENV') === 'production';
    const ctx = host.switchToHttp();
    const response = ctx.getResponse();
    
    if (isProduction) {
      // 프로덕션 환경에서는 일반화된 오류 메시지 제공
      return response.status(500).json({
        statusCode: 500,
        message: '서버 오류가 발생했습니다.'
      });
    }
    
    // 개발 환경에서는 상세 오류 정보 제공
    const errorDetails = exception instanceof Error
      ? exception.stack
      : String(exception);
      
    response.status(500).json({
      statusCode: 500,
      message: '서버 오류가 발생했습니다.',
      details: errorDetails
    });
  }
}
```


## 예외 레이어와 로깅 통합

- 예외 레이어를 로깅 시스템과 통합하여 모든 예외를 기록할 수 있다.

```typescript
@Catch()
export class LoggingExceptionFilter implements ExceptionFilter {
  constructor(private readonly logger: Logger) {}

  catch(exception: any, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const request = ctx.getRequest();
    
    this.logger.error(
      `예외 발생: ${exception.message}`,
      exception.stack,
      `${request.method} ${request.url}`
    );
    
    // 예외 처리 계속 진행
    throw exception;
  }
}
```


## 예외 레이어의 이점

- **일관성**: 애플리케이션 전체에서 일관된 예외 처리 및 응답 형식을 제공한다.
- **중앙화**: 모든 예외 처리 로직을 한 곳에서 관리할 수 있다.
- **유연성**: 다양한 종류의 예외에 대해 맞춤형 처리가 가능하다.
- **보안**: 민감한 오류 정보를 적절히 감출 수 있다.
- **로깅 통합**: 모든 예외를 자동으로 기록하고 모니터링할 수 있다.


## 모범 사례

1. **도메인별 예외 정의**
   - 비즈니스 규칙과 관련된 구체적인 예외 클래스를 생성한다.
   - 이를 통해 코드의 가독성이 향상되고 예외의 의미가 명확해진다.

2. **예외 필터 계층화**
   - 전역 예외 필터를 설정하여 기본적인 예외 처리를 제공한다.
   - 특정 도메인이나 컨트롤러에 대해 더 구체적인 필터를 적용한다.

3. **환경별 처리 분리**
   - 개발 환경에서는 상세한 오류 정보를 제공한다.
   - 프로덕션 환경에서는 일반화된 메시지와 로깅에 중점을 둔다.

4. **적절한 HTTP 상태 코드 사용**
   - 각 예외 유형에 맞는 적절한 HTTP 상태 코드를 사용한다.
   - 클라이언트 오류(4xx)와 서버 오류(5xx)를 명확히 구분한다.

5. **오류 응답 형식 표준화**
   - 모든 오류 응답이 일관된 구조를 가지도록 한다.
   - 상태 코드, 메시지, 타임스탬프, 요청 경로 등의 표준 필드를 포함한다.

- [[NestJS]]의 예외 레이어는 강력하고 유연한 예외 처리 시스템을 제공하여 애플리케이션의 안정성과 사용자 경험을 크게 향상시킨다.
