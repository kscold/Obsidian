- [[인터셉터(Interceptor)]]를 사용하려면 @UseInterceptors() [[데코레이터(Decorator)]]를 사용하여 [[컨트롤러(Controller)]] 또는 [[핸들러(Handler)]]에 등록해야 한다.

- 즉, @UseInterceptors() [[데코레이터(Decorator)]]가 [[의존성 주입(Dependency Injection)]]을 해주는 것이다.


## 문법

- 아래 코드와 같이 [[컨트롤러(Controller)]] 위에 @UseInterceptors() [[데코레이터(Decorator)]]로 [[의존성 주입(Dependency Injection)]]을 한다.

```ts
import { UseInterceptors } from '@nestjs/common';  
import { TestInterceptor } from '../common/interceptors/test.interceptor';  
  
@UseInterceptors(TestInterceptor)  
@Controller()
export class TestController {  
	...
}
```