- [[데코레이터(Decorator)]]는 비즈니스와 상관 없는 로직들을 숨기면서 기능을 변경하거나 확장할 수 있다.
- 여러 [[클래스(class)]]에서 반복되는 공통 관심사가 있을 때 [[데코레이터(Decorator)]]를 사용하면 중복된 코드를 줄이고 코드를 [[NestJS 모듈(module)]] 단위로 관리하는 효과를 거둘 수 있다.

- 커스텀 데코레이터는 [[createParamDecorator()]] [[메서드(Method)]]를 이용하여 선언한다.

![](https://static.toss.im/ipd-tcs/toss_core/live/82661683-cc4e-48d6-b4b9-8e233c9ec3d9/tech-blog-1-1024x847.png)


## 예시

- 커스텀 데코레이터를 만들 때, 파일 이름은 통상적으로 이름.decorator.ts로 선언한다.
- 아래는 [[req]].user로 사용하지 않고 user로 데이터를 바로 추출할 수 있도록 만드는 [[데코레이터(Decorator)]]이다.


```ts
import { createParamDecorator, ExecutionContext } from '@nestjs/common';  
import { User } from './user.entity';  
  
export const GetUser = createParamDecorator((data, ctx: ExecutionContext): User => {  
    const req = ctx.switchToHttp().getRequest();  
    return req.user;  
});
```