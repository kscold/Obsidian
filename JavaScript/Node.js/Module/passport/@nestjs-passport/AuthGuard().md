- [[@UseGuards()]]안에 @nestjs/passport에서 가져온 AuthGuard()를 이용하면 요청 안에 유저 정보를 넣어줄 수 있다.
- AuthGuard를 [[extends]]하여 [[가드(Guard)]]를 만들 때에 [[CanActivate]]를 [[implements]]를 안해주는 이유는 AuthGauard안에 [[CanActivate]]를 상속하고 있기 때문이다.


## 문법

- 따라서 아래와 같이 canActivate [[메서드(Method)]]를 정의해주면 된다.

```ts
import { ExecutionContext, Injectable } from '@nestjs/common';  
import { AuthGuard } from '@nestjs/passport';  

@Injectable()  
export class 가드이름 extends AuthGuard('local') {  
    async canActivate(context: ExecutionContext): Promise<boolean> { 
	    ... 
	}  
}
```