- 리플렉터(Reflector)는 이러한 메타데이터([[메타데이터(Metadata)]])를 읽고 조작할 수 있게 해주는 [[NestJS]]의 유틸리티 [[클래스(class)]]이다.


## [[메서드(Method)]]

### get()

- 메타데이터([[메타데이터(Metadata)]]) key를 사용하여 값을 조회한다.
### getAllAndOverride()

- 여러 [[스코프(Scope)]]에서 메타데이터([[메타데이터(Metadata)]])를 조회하고 오버라이드한다.

### getAllAndMerge()

- 여러 [[스코프(Scope)]]의 메타데이터([[메타데이터(Metadata)]])를 병합한다.


## 예시

- 아래는 [[커스텀 가드(Custom Garud)]]

```ts
@Injectable()
export class RBACGuard implements CanActivate {
    constructor(private readonly reflector: Reflector) {}
	
    canActivate(context: ExecutionContext): boolean {
        // RBAC 데코레이터로 설정된 role 메타데이터를 가져옴
        const requiredRole = this.reflector.get<Role>(RBAC, context.getHandler());
		
        // role이 설정되지 않은 경우 접근 허용
        if (!Object.values(Role).includes(requiredRole)) {
            return true;
        }
		
        const request = context.switchToHttp().getRequest();
        const user = request.user;
		
        // 사용자가 없는 경우 접근 거부
        if (!user) {
            return false;
        }
		
        // 사용자의 role이 필요한 role과 일치하는지 확인
        return user.role === requiredRole;
    }
}
```

리플렉터의 작동 방식:

- 데코레이터를 통해 메타데이터 저장

- 가드에서 리플렉터로 메타데이터 조회

- 조회된 메타데이터를 기반으로 접근 제어 로직 실행

이렇게 리플렉터를 사용하면 선언적으로 접근 제어를 구현할 수 있으며, 코드의 재사용성과 유지보수성이 향상됩니다.