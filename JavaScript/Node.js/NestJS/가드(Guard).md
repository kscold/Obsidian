- 가드(Guard)의 주요 목적은 보통 [[컨트롤러(Controller)]]에 접근하기 전의 권한을 체크(401, 403)는 것에 주로 쓰인다.
- 물론 데이터 자체도 제대로 체크할 수 있지만, 이런 기능은 [[class-validator]]가 많이 대신한다.
- [[NestJS 미들웨어(Middleware)]]

- 가드(Guard)는 [[@Injectable()]] [[데코레이터(Decorator)]]를 사용하며 [[CanActivate]] 인터페이스([[interface]])를 구현한 [[클래스(class)]]이다.

- [[@Injectable()]]를 사용한 이유는 [[인스턴스(Instance)]] 대신 타입을 전달하여 사용하고, [[인스턴스(Instance)]]화에 대한 책임은 프레임워크에 남겨두고 [[의존성 주입(Dependency Injection)]]을 가능하게 하기 위해서이다. 

- 대표적으로 @nestjs/passport의 [[Passport Module]]을 사용한 [[AuthGuard()]]가 있다.


## 가드의 역할

- 가드는 단일 책임을 가지며 특정한 상황들(permissions, role, ACLs 등)에 따라, 주어진 request가 라우트 핸들러에 의해 처리 여부를 결정한다. 
- [[Express]]에서는 주로 [[미들웨어(Middleware)]]로 처리를 하였다.

![](https://docs.nestjs.com/assets/Guards_1.png)  

- [[NestJS 미들웨어(Middleware)]]에서는 주로 인증에 관련된 작업을 사용할 때 사용하며 [[NestJS]]에서 인가에 관련된 작업은 가드를 통해 이뤄진다.

- 공식문서에 의하면 [[미들웨어(Middleware)]]는 [[next()]]가 호출 된 후 어떠한 라우트 핸들러가 실행될 지를 모른다.
- 하지만 가드는 [[ExecutionContext]]를 사용할 수 있기 때문에 다음에 어떠한 라우트 핸들러가 실행되는지 정확하게 알 수 있다.