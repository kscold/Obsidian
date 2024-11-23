- [[NestJS]]의 [[인터셉터(Interceptor)]]를 작성하기 위해서는 [[@Injectable()]] [[데코레이터(Decorator)]]를 통한 [[클래스(class)]]를 작성해주어야한다.

- 이 때 해당 [[클래스(class)]]는 [[NestJS]]의 NestInterceptor를 [[상속(Inheritance)]]해야하며, [[클래스(class)]] 내부적으로는 interceptor [[메서드(Method)]]를 포함해야한다. 


## NestInterceptor 상속 문법

- 이 때 작성되는 interceptor() [[메서드(Method)]]의 인자로, [[ExecutionContext]]와 [[CallHandler]]가 포함된다. 
- 기본적으로 작성되어야 할 코드는 다음과 같다.

```ts
import { CallHandler, ExecutionContext, Injectable, NestInterceptor } from "@nestjs/common";
import { Observable } from "rxjs";

@Injectable()
export class TestInterceptor implements NestInterceptor {
    intercept(context: ExecutionContext, next: CallHandler<any>): Observable<any> | Promise<Observable<any>> {
        
        // 핸들러 도달 전 인터셉터 로직
        return next.handle().pipe(
            // 핸들러 도달 이후 인터셉터 로직
        )
        
    }
}
```