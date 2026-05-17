- 컨트롤러(Controller)는 MVC 패턴에서 클라이언트의 요청을 수신하고, [[서비스(Service)]]에 처리를 위임한 뒤 응답을 반환하는 계층이다.
- 비즈니스 로직을 직접 포함하지 않고, 요청 파싱과 응답 반환만 담당하는 것이 원칙이다.

## 역할

- 클라이언트 HTTP 요청 수신 (GET, POST, PUT, DELETE 등)
- 요청 파라미터·바디 파싱 후 [[서비스(Service)]]에 전달
- [[서비스(Service)]]가 반환한 결과를 HTTP 응답으로 클라이언트에 반환
- [[라우트 핸들러(Route Handler)]]를 정의하여 URL 경로와 메서드를 매핑

## Express에서의 컨트롤러

- Express는 별도의 컨트롤러 클래스가 없으므로 함수 형태로 분리하는 것이 관례다.

```js
// userController.js
const UserService = require('./userService');

const getUser = async (req, res) => {
  const user = await UserService.findById(req.params.id);
  if (!user) return res.status(404).json({ message: '사용자를 찾을 수 없습니다.' });
  res.json(user);
};

module.exports = { getUser };
```

```js
// userRouter.js
const express = require('express');
const { getUser } = require('./userController');

const router = express.Router();
router.get('/:id', getUser); // 라우트 핸들러 연결
module.exports = router;
```

## NestJS에서의 컨트롤러

- NestJS에서는 `@Controller()` [[데코레이터(Decorator)]]로 클래스를 컨트롤러로 선언한다.
- 메서드에는 `@Get()`, `@Post()` 등 HTTP 메서드 데코레이터를 붙인다.
- 비즈니스 로직은 반드시 [[서비스(Service)]]로 위임한다.

```ts
import { Controller, Get, Post, Param, Body } from '@nestjs/common';
import { UserService } from './user.service';
import { CreateUserDto } from './dto/create-user.dto';

@Controller('users') // /users 경로에 매핑
export class UserController {
  constructor(private readonly userService: UserService) {}

  @Get(':id') // GET /users/:id
  async getUser(@Param('id') id: string) {
    return this.userService.findById(id);
  }

  @Post() // POST /users
  async createUser(@Body() dto: CreateUserDto) {
    return this.userService.create(dto);
  }
}
```

## [[모델(Model)]]과의 관계

- 컨트롤러는 [[모델(Model)]]에 직접 접근하지 않는다.
- `컨트롤러 → [[서비스(Service)]] → [[모델(Model)]]` 흐름으로 데이터에 접근한다.

## 컨트롤러 설계 원칙

- 컨트롤러 메서드는 짧게 유지 (파라미터 바인딩 + 서비스 호출 + 결과 반환만)
- 정책 판단, 복잡한 조립 로직은 [[서비스(Service)]]나 유스케이스로 분리
- 동일 도메인이라도 역할(query/command)별로 컨트롤러 파일을 분리하는 것이 좋다
