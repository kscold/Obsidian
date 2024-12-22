- [[NestJS]]에서는 [[가드(Guard)]]를 만들기 위해서는 `@nestjs/common` [[모듈(Module)]]에서 제공하는 CanActivate라는 인터페이스([[Interface]])를 구현하는 [[클래스(class)]]를 생성해야 한다. 

- 그리고 canActivate() [[메서드(Method)]]를 안에 [[가드(Guard)]]의 로직을 작성할 수 있다.

- CanActivate는 [[가드(Guard)]]에서 [[상속(Inheritance)]]하는 인터페이스([[interface]])이다.


## CanActivate 구조

- CanActive 인터페이스([[interface]])의 구조는 아래와 같다.

```ts
export interface CanActivate {
    canActivate(context: ExecutionContext): boolean | Promise<boolean> | Observable<boolean>;
}
```

- canActivate()라는 [[메서드(Method)]]가 있으며 [[매개변수(parameter)]]로 [[실행 컨텍스트(execution context)]]([[ExecutionContext]])를 받고 있다.


## 예시

- 아주 간단한 예시로 canActivate() [[메서드(Method)]]가 항상 true를 반환하도록 구현해본다.

```ts
import { Injectable, CanActivate } from '@nestjs/common';

@Injectable()
export class AuthGuard implements CanActivate {
	canActivate() {
	    console.log('인증이 성공하였습니다.');
		return true;
	}
}
```

- [[NestJS]]는 canActivate() [[메서드(Method)]]가 true 또는 [[Promise<>]]를 반환했을 때만 해당 요청을 [[컨트롤러(Controller)]]로 전달한다.
- 반대로 canActivate() [[메서드(Method)]]가 false 또는 `Promise<false>`를 반환할 경우에는 해당 요청이 [[컨트롤러(Controller)]]로 넘어가는 것을 차단한다.