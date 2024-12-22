- 메타데이터는 '데이터에 대한 데이터'를 의미한다.
- [[NestJS]]에서는 [[클래스(class)]], [[메서드(Method)]], [[속성(Property)]] 등에 추가 정보를 첨부할 수 있게 해주는 장치이다.

- [[NestJS]]에서 Meatadata는 루트 [[핸들러(Handler)]]에 첨부되며, 이 Metadata에 접근하면 특정한 결정을 내릴 수 있는 기능을 구현할 수 있다.

- Role에 따른 접근 권한 구현을 대표적인 예시로 들 수 있다.


## 문법

```ts
@Post() // HTTP 메서드에 대한 메타데이터
@SetMetadata('roles', ['admin']) // ['admin']은 권한에 대한 메타데이터
async create(@Body() createCatDto: CreateCatDto) {
	...
}
```

- 기본적으로 Metadata는 [[컨트롤러(Controller)]]에서 사용된다.
- [[@SetMetadata()]] [[데코레이터(Decorator)]]를 통해 사용할 수 있다.
- Metadata내의 'roles'라는 키가 존재하고, 그 roles중 'admin'의 값을 가진 경우에만 [[컨트롤러(Controller)]]를 통과할 수 있다.


## 커스텀 데코레이터 선언으로 사용

- 공식문서에서는 코드 가독성 및 재사용성을 높이기 위해 아래와 같이 [[커스텀 데코레이터(Custom Decorator)]]를 만들어서 구현하는 것을 권장하고 있다.

```ts
//roles.decorator.ts
import { SetMetadata } from '@nestjs/common';

type UserRole = 'guest' | 'admin' | 'user' | 'any'

export const Roles = (roles: UserRole[]) => SetMetadata('roles', roles);
```

- type으로 특정 텍스트를 지정해서 제약조건을 만들 수 있다.

```ts
@Post()
@Roles('admin')
async create(@Body() createCatDto: CreateCatDto) {
	...
}
```

- [[커스텀 데코레이터(Custom Decorator)]]로 코드의 가독성이 개선된 것을 볼 수 있다.


## [[가드(Guard)]]와 함께 사용

- 위에서 설정한 Metadata를 이용하여 권한관리 등을 하려면, [[가드(Guard)]]와 함께 사용해야 한다.

```ts
@Injectable()
export class AuthGuard implements CanActivate {
	constructor(private readonly reflector: Reflector) {}
	
	canActivate(context: ExecutionContext) {
	    //metadata에 저장한 키로 reflector에서 불러오기
		const roles = this.reflector.get<AllowedRoles>('roles', context.getHandler());
	    
	    if (!roles) {
		    return true;
		}
	    
	    const getContext = GqlExecutionContext.create(context).getContext();
	    const user = getContext['user'];
	    
	    if (!user) {
		    return false;
	    }
	    
	    if (roles.includes('Any')) {
		    return true;
		}
		
	    return roles.includes(user.role);
	}
}
```

```typescript
//auth.module.ts 
import { Module } from '@nestjs/common';
import { APP_GUARD } from '@nestjs/core';
import { AuthGuard } from './auth.guard';

@Module({
  //guard를 app모든 곳에서 사용하고 싶다면
  providers: [
    {
      provide: APP_GUARD,
      useClass: AuthGuard,
    },
  ],
})
export class AuthModule {}
```

```typescript
//app.module.ts
@Module({
  ...
  imports : [
    ...
    AuthModule,
    ...
  ]
  ...
})
```

- 기존에 생성한 AuthGuard와 함께 구현할 수 있다.
- [[Reflector]]를 불러와서 지정한 key를 통해 값에 접근할 수 있다.
- 위의 예시들과 같이 구현하면, 지정된 role이 아닌 사용자가 create에 접근할 경우 guard에서 forbidden을 띄우고 접근을 차단한다.