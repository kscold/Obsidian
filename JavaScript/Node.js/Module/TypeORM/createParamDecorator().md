- createParamDecorator()는 개발자가 정의한 [[커스텀 데코레이터(Custom Decorator)]]를 생성하는 [[메서드(Method)]]이다. 

- createParamDecorator()는 [[컨트롤러(Controller)]] [[메서드(Method)]]의 인자로 주어지는 [[매개변수(parameter)]]를 변경하거나, 추가적인 작업을 수행하는 데 사용된다.

- 인자로는 [[콜백 함수(Callback Function)]]를 받는다.
- 이 [[콜백 함수(Callback Function)]]는 해당 [[매개변수(parameter)]]에서 추출하려는 데이터를 반환한다.
 ​

## 문법

- context([[ctx]])의 타입은 [[ExecutionContext]]를 받고, [[switchToHttp()]].getRequest()를 통해 들어오는 요청을 정확히 판단한다.

```ts
export const 데코레이터명 = createParamDecorator((data: unknown, context: ExecutionContext) => {
	const req: { } = context.switchToHttp().getRequest(); 
	return req; 
});
```

### data 

- createParamDecorator에 전달되는 데이터이다.
### context 

- 현재 실행 중인 요청과 관련된 데이터를 추출한다.
- 예를 들어 @User를 사용하면 현재 요청된 (접속한) 사용자 객체(req.user)를 반환한다.


## [[컨트롤러(Controller)]]에서 [[커스텀 데코레이터(Custom Decorator)]] 사용

- 아래 코드는 먼저 [[커스텀 데코레이터(Custom Decorator)]]를 선언하는 부분이다.

```ts
// src/auth/auth.decorator.ts 
import { createParamDecorator, ExecutionContext } from '@nestjs/common';
import { UserEntity } from '../entities/user.entity';

export const User = createParamDecorator((data: unknown, context: ExecutionContext) => {
	const req: { user?: UserEntity } = context.switchToHttp().getRequest();
	return req.user;
});

export const UserName = createParamDecorator((data: unknown, context: ExecutionContext) => {
	const req: { user?: UserEntity } = context.switchToHttp().getRequest();
	return req.user.name;
});

export const UserId = createParamDecorator((data: unknown, context: ExecutionContext) => {
	const req: { user?: UserEntity } = context.switchToHttp().getRequest();
	return Number(req.user.id);
});
```

- 생성된 [[커스텀 데코레이터(Custom Decorator)]]는 [[컨트롤러(Controller)]]에서 다음과 같이 사용할 수 있다.

```ts
// src/resource/user/user.controller.ts 
import { Body, Controller, Get, Post, Req, UseGuards } from '@nestjs/common'; import { UserService } from './user.service'; 
import { CreateUserDto, LoginUserDto } from './dtos/create-user.dto';
import { ApiBody, ApiOperation, ApiTags } from '@nestjs/swagger'; 
import { LocalServiceAuthGuard } from '../../auth/guards/local-service.guard';
import { AuthService } from '../../auth/auth.service'; 
import { JwtServiceAuthGuard } from '../../auth/guards/jwt-service.guard'; import { User } from '../../auth/auth.decorator'; 
import { UserEntity } from '../../entities/user.entity'; 

@ApiTags('사용자 API') 
@Controller('user') 
export class UserController { 
	constructor(private userService: UserService, private authService: AuthService) {} 
	@ApiOperation({ summary: '사용자 생성 API', description: '사용자를 생성한다.' })
	@ApiBody({ type: CreateUserDto })
	@Post('join') 
	async postJoin(@Body() createUserDto: CreateUserDto) {
		...
	} 
	
	// @User() 적용 
	@ApiOperation({ summary: '사용자 로그인 API', description: '사용자가 로그인을 한다.' }) 
	@UseGuards(LocalServiceAuthGuard)
	@ApiBody({ type: LoginUserDto })
	@Post('login') async postLogin(@User() user: UserEntity, @Body() loginUserDto: LoginUserDto) { 
		const token = this.authService.loginServiceUser(user); 
		return token; 
	}
	
	@ApiOperation({ summary: '내 정보 조회 API', description: '이름, 메일 등을 조회한다.' })
	@UseGuards(JwtServiceAuthGuard) 
	@Get('profile') async getProfile() {
		...
	} 
}

```
