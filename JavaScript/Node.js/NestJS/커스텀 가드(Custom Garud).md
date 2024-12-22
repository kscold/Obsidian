- [[NestJS]]에서 커스텀 가드는 특정 조건이나 비즈니스 로직에 따라 요청의 처리 여부를 결정하는 사용자 정의 [[가드(Guard)]]이다.


## 문법

- [[@Injectable()]] [[데코레이터(Decorator)]]를 사용하고 [[CanActivate]] 인터페이스([[Interface]])를 구현하는 [[클래스(class)]]를 만들어야 한다.

- canActivate() [[메서드(Method)]]를 반드시 구현해야 한다.

```ts
@Injectable()
export class CustomGuard implements CanActivate {
    canActivate(
        context: ExecutionContext,
    ): boolean | Promise<boolean> | Observable<boolean> {
        const request = context.switchToHttp().getRequest();
        
        // 여기에 커스텀 로직을 구현
        
        return true;
    }
}
```


## 커스텀 가드의 주요 특징

- [[ExecutionContext]]를 통해 현재 실행 중인 [[핸들러(Handler)]]와 [[클래스(class)]]에 대한 정보에 접근할 수 있다.
- 메타데이터([[메타데이터(Metadata)]])를 활용하여 더 유연한 권한 제어가 가능하다.
- [[의존성 주입(Dependency Injection)]]을 통해 다른 [[서비스(Service)]]나 프로바이터([[프로바이더(Provider)]])를 사용할 수 있다.
- [[동기(Synchronous)]] 또는 [[비동기(asynchronous)]]적으로 처리가 가능하다.


## 예시

- 아래는 역할 기반 [[가드(Guard)]]를 구현하는 예시 코드이다.

```ts@
Injectable()
export class RolesGuard implements CanActivate {
    constructor(private reflector: Reflector) {}
	
    canActivate(context: ExecutionContext): boolean {
        const roles = this.reflector.get<string[]>('roles', context.getHandler());
	    
		if (!roles) {
            return true;
        }
        
        const request = context.switchToHttp().getRequest();
        const user = request.user;
        
        return roles.includes(user.role);
    }
}
```


## 에러 처리

- 커스텀 가드에서는 [[HttpException]]을 [[throw]]하여 클라이언트에게 적절한 [[에러(error)]] 응답을 전달할 수 있다.

```ts
if (!user) {
	throw new UnauthorizedException('인증되지 않은 사용자입니다.');    
}
```
