NestJS에서 기본 제공되는 필터 외에 사용자가 직접 정의한 필터이다.
애플리케이션의 특정 요구사항에 맞게 예외 처리 로직을 커스터마이즈할 수 있다.
[[ExceptionFilter]] 인터페이스를 구현하여 만든다.
특정 예외 타입에 대해 맞춤형 응답을 제공할 수 있다.
로깅, 에러 변환, 특별한 응답 형식 등을 구현할 수 있다.

커스텀 필터 구현 방법

@Catch() 데코레이터는 어떤 타입의 예외를 처리할지 지정한다.
여러 예외 타입을 처리하려면 @Catch(ExceptionType1, ExceptionType2)와 같이 지정한다.
catch() 메서드에서 예외 처리 로직을 구현한다.

```ts
@Catch(HttpException)
export class LoggingExceptionFilter implements ExceptionFilter {
  constructor(private readonly logger: Logger) {}

  catch(exception: HttpException, host: ArgumentsHost) {
    const ctx = host.switchToHttp()
    const response = ctx.getResponse()
    const request = ctx.getRequest()
    const status = exception.getStatus()

    this.logger.error(
      `HTTP 상태: ${status}, 요청 URL: ${request.url}`,
      exception.stack,
      "ExceptionFilter"
    )

    response.status(status).json({
      statusCode: status,
      timestamp: new Date().toISOString(),
      path: request.url,
    })
  }
}
```

@Catch() 데코레이터는 어떤 타입의 예외를 처리할지 지정한다.
여러 예외 타입을 처리하려면 @Catch(ExceptionType1, ExceptionType2)와 같이 지정한다.
catch() 메서드에서 예외 처리 로직을 구현한다.
커스텀 필터 적용 방법
컨트롤러 레벨 적용
}
메서드 레벨 적용
}
글로벌 레벨 적용
}
사용자 정의 예외
}
사용자 정의 예외는 HttpException 클래스를 상속받아 만들 수 있다.
비즈니스 로직에 맞는 커스텀 예외를 정의하여 코드의 가독성을 높일 수 있다.
표준 HTTP 예외와 일관된 방식으로 처리된다.
표준 HTTP 예외와 일관된 방식으로 처리된다.
고급 필터 기능
예외 클래스의 실제 타입을 확인하여 다양한 예외 유형에 대해 다르게 처리할 수 있다.
의존성 주입을 통해 서비스나 로거를 필터에 주입할 수 있다.
여러 필터를 함께 사용하여 복잡한 예외 처리 체인을 구성할 수 있다.
예외 정보를 로그로 남기거나 외부 서비스에 보고할 수 있다.
}
커스텀 필터를 사용하면 애플리케이션 전체에서 일관된 예외 처리 방식을 구현할 수 있다.
비즈니스 로직과 예외 처리 로직을 분리하여 코드의 가독성과 유지보수성을 높일 수 있다.
다양한 클라이언트 환경에 맞춘 응답 형식을 제공할 수 있다.

@Controller('products')
@UseFilters(CustomExceptionFilter)
export class ProductsController {
// 컨트롤러 내 모든 라우트에 필터 적용
}
