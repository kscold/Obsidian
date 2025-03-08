- 역할(Role) 기반으로 권한을 나눠서 특정 리소스에 [[CRUD]] 작업을 할 수 있는지 여부를 결정한다.

- 역할(Role 기반으로 권한(permission)을 나눠서 특정 리소스에 [[CRUD]] 작업을 할 수 있는지 여부를 결정하는 접근 제어(Access Control) 방식이다.
- 사용자에게 역할을 부여하고, 각 역할에 따른 권한을 관리한다.


## 주요 구성 요소
### 사용자(User)

- 시스템을 사용하는 개체이다.
### 역할(Role)

- 사용자에게 부여되는 직무나 책임이다.
- 일반적인 역할 구조는 ADMIN(모든 권한을 가진 관리자), (USER)일반적인 사용 권한을 가진 사용자, GUEST(제한된 읽기 권한만 가진 방문자)형식이다.
### 권한(Permission)

- 특정 리소스에 대한 접근 권한이다.
### 리소스(Resource)

- 접근을 제어하고자 하는 시스템의 자원이다.


## 예시

```ts
enum Role {
    ADMIN = 'admin',
    USER = 'user',
    GUEST = 'guest'
}

@Injectable()
export class RolesGuard implements CanActivate {
    canActivate(context: ExecutionContext): boolean {
        const requiredRoles = this.reflector.get<Role[]>('roles', context.getHandler());
        const user = context.switchToHttp().getRequest().user;
        
        return requiredRoles.some((role) => user.roles.includes(role));
    }
}
```

- 권한 관리가 단순화되고 체계적이다.
- 사용자 권한을 쉽게 변경할 수 있다.
- 보안 정책을 중앙에서 관리할 수 있다.

- 단점은 역할이 많아지면 관리가 복잡해질 수 있으며 세분화된 권한 제어가 필요한 경우 추가적인 설정이 필요하다.