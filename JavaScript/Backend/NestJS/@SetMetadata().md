- `@SetMetadata()`는 [[NestJS]]에서 [[라우트 핸들러(Route Handler)]]나 컨트롤러 클래스에 임의의 [[메타데이터(Metadata)]]를 붙이는 [[데코레이터(Decorator)]]이다.
- 붙인 메타데이터는 `Reflector`를 통해 [[가드(Guard)]]나 [[인터셉터(Interceptor)]] 안에서 읽어 사용한다.

## 문법

```ts
@SetMetadata('key', value)
```

- 첫 번째 인자: 메타데이터를 식별하는 키(string)
- 두 번째 인자: 저장할 값 (원시값, 배열, 객체 모두 가능)

## 기본 사용 예시

```ts
import { SetMetadata } from '@nestjs/common';

// 라우트에 roles 메타데이터를 붙임
@SetMetadata('roles', ['admin', 'breeder'])
@Get('protected')
protectedRoute() {
  return 'admin or breeder only';
}
```

## 커스텀 데코레이터로 래핑하는 패턴 (권장)

- `@SetMetadata()`를 직접 사용하면 키 문자열이 분산되기 쉽다.
- 상수 키와 함께 커스텀 데코레이터로 래핑하는 것이 표준 패턴이다.

```ts
// roles.decorator.ts
import { SetMetadata } from '@nestjs/common';

export const ROLES_KEY = 'roles'; // 키를 상수로 관리

export const Roles = (...roles: string[]) => SetMetadata(ROLES_KEY, roles);
```

```ts
// 컨트롤러에서 커스텀 데코레이터 사용
@Roles('admin')
@Get('admin-only')
adminRoute() {
  return 'admin only';
}
```

## Reflector로 메타데이터 읽기 ([[가드(Guard)]]에서)

```ts
// roles.guard.ts
import { Injectable, CanActivate, ExecutionContext } from '@nestjs/common';
import { Reflector } from '@nestjs/core';
import { ROLES_KEY } from './roles.decorator';

@Injectable()
export class RolesGuard implements CanActivate {
  constructor(private reflector: Reflector) {}

  canActivate(context: ExecutionContext): boolean {
    // 핸들러 또는 클래스에서 roles 메타데이터를 읽음
    const requiredRoles = this.reflector.getAllAndOverride<string[]>(ROLES_KEY, [
      context.getHandler(), // 메서드 레벨 먼저
      context.getClass(),   // 클래스 레벨 폴백
    ]);

    if (!requiredRoles) return true; // 메타데이터 없으면 통과

    const { user } = context.switchToHttp().getRequest();
    return requiredRoles.includes(user.role);
  }
}
```

## 활용 사례

- 역할 기반 접근 제어 (RBAC) — `@Roles('admin')`
- 공개 라우트 표시 — `@Public()` (`@SetMetadata('isPublic', true)`)
- 권한 범위(scope) 설정 — `@Permissions('read:users')`
