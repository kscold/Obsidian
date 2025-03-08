- [[NestJS]]에서 인터셉터는 문자 그대로 가로채다라는 의미를 갖고 있다.
- [[AOP(Aspect-Oriented Programming)]] 개념을 구현하는 핵심 요소이다.

- 즉, 특정 작업을 수행하기 전이나 후에 추가 로직을 실행할 수 있는 코드 블록이다.
- [[NestJS]]에서 유일하게 요청이 들어올 때 그리고 나갈 때 모두 로직을 실행 할 수 있는 [[미들웨어(Middleware)]]이다.
- 전과 후 둘 다 적용시켜도 되고, 전이나 후에 단일로 적용시켜도 된다.
- [[리액트(React)]]의 [[useEffect()]]를 생각하면 쉽다.

- 주로 [[컨트롤러(Controller)]]에 적용되고 [[컨트롤러(Controller)]]에서 반환되는 데이터를 다시 한번 가공할 수도 있다.

- 특히, [[HTTP(Hyper Tranfer Protocol)]] 요청과 응답을 처리할 때 특히 유용하다.
- 주로 로깅, [[에러(error)]] 처리, 데이터 변환 및 인증과 같은 공통 관심사를 처리하는 데 사용된다.


## 인터셉터 내부 동작과정

- 요청 인터셉터의 경우 전역(Global), [[컨트롤러(Controller)]], 라우트(Route) 방식으로 들어가고 응답 인터셉터의 경우 라우트(Route), [[컨트롤러(Controller)]],전역(Global) 순으로 동작한다.

![[Pasted image 20250301181155.png]]


## 인터셉터의 특징

- 함수 실행 전/후에 추가 로직을 [[바인딩(binding)]]한다.
- 함수에서 변환된 값을 반환한다.
- 함수에서 던진 에러를 변환한다.
- 함수의 기본 기능에 추가 기능을 연장한다.
- 조건에 따라 함수의 기능을 override한다.
- 인터셉터 응답 핸들링은 기본적으로 [[RxJS(Reactive Extensions For JavaScript)]]를 사용한다.


## 인터셉터 구현

- 인터셉터도 [[@Injectable()]] [[데코레이터(Decorator)]]를 통해 [[의존성 주입(Dependency Injection)]]을 받는다.
- 이후 [[implements]]를 통해 [[NestInterceptor]] 인터페이스([[interface]])를 [[상속(Inheritance)]]받는 것이 안전하다.

- 웹스톰에서는 [[NestInterceptor]]를 [[상속(Inheritance)]]받으면 intercept [[메서드(Method)]]는 자동으로 구현된다.

```ts
import {  
    CallHandler,  
    ExecutionContext,  
    Injectable,  
    NestInterceptor,  
} from '@nestjs/common';  
import { Observable } from 'rxjs';  
import { map } from 'rxjs/operators';  
  
@Injectable()  
export class UndefinedToNullInterceptor implements NestInterceptor {  
    intercept(  
        context: ExecutionContext,  
        next: CallHandler<any>,  
    ): Observable<any> | Promise<Observable<any>> {  
        // 핸들러 도달 전 인터셉터 로직
		  
        // 핸들러 도달 이후 인터셉터 로직
        return next  
            .handle()  
            .pipe(map((data) => (data === undefined ? null : data)));  
    }  
}
```

- 위의 코드 부분에서 return 부분은 [[핸들러(Handler)]] 도달 이후에 응답을 보내기 직전에 시작된다.
- 인터셉터에서 [[에러(error)]]를  처리해야할 때에는 [[catchError]]를 사용한다.

- 정의한 인터셉터를 사용하기 위해서는 [[@UseInterceptors()]] [[데코레이터(Decorator)]]를 사용하면 된다.