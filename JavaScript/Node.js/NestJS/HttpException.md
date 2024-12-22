- [[NestJS]]에서 HttpException은 [[HTTP(Hyper Tranfer Protocol)]] 기반 애플리케이션에서 발생하는 다양한 HTTP 오류를 처리하기 위한 내장 예외 [[클래스(class)]]이다.


## 문법

- HttpException은 message와 statusCode를 [[매개변수(parameter)]]로 받는다.

```ts
throw new HttpException('message', statusCode);
```


## 내장된 HTTP 예외들

- [[NestJS]]는 기본적인 HttpException을 [[상속(Inheritance)]]하는 다양한 예외 [[클래스(class)]]를 제공한다.

### BadRequestException()

- 400오류이다.

```ts
throw new BadRequestException('잘못된 요청입니다.');
```

- UnauthorizedException (401)
    
    throw new UnauthorizedException('인증되지 않은 요청입니다.');
    

- ForbiddenException (403)
    
    throw new ForbiddenException('접근 권한이 없습니다.');
    

- NotFoundException (404)
    
    throw new NotFoundException('리소스를 찾을 수 없습니다.');
    

5. ConflictException (409)

throw new ConflictException('리소스가 이미 존재합니다.');

## 커스텀 예외 응답

- 응답 본문을 커스터마이징할 수 있다.
    
    throw new HttpException({
    
        status: HttpStatus.FORBIDDEN,
    
        error: '커스텀 에러 메시지',
    
        details: '추가적인 에러 정보'
    
    }, HttpStatus.FORBIDDEN);


## 필터와 함께 사용

- 예외 [[커스텀 필터(Filter)]]를 통해 예외 처리 로직을 중앙화할 수 있다.
    
    @Catch(HttpException)
    
    export class HttpExceptionFilter implements ExceptionFilter {
    
        catch(exception: HttpException, host: ArgumentsHost) {
    
            const ctx = host.switchToHttp();
    
            const response = ctx.getResponse();
    
            const status = exception.getStatus();
    
            response.status(status).json({
    
                statusCode: status,
    
                timestamp: new Date().toISOString(),
    
                message: exception.message,
    
            });
    
        }
    
    }
    

## 상태 코드

- HTTP 상태 코드는 [[HttpStatus]] enum을 통해 제공된다.
    
    import { HttpStatus } from '@nestjs/common';
    
    throw new HttpException('메시지', HttpStatus.BAD_REQUEST);