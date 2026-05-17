- `@Req()` 데코레이터는 [[NestJS]] 컨트롤러 메서드 파라미터에 붙여 [[req]] 객체(Express의 Request 객체)를 직접 주입받을 때 사용한다.
- `@Request()`와 완전히 동일한 별칭(alias)이다.

## 문법

```ts
import { Controller, Get, Req } from '@nestjs/common';
import { Request } from 'express';

@Controller('example')
export class ExampleController {
  @Get()
  getExample(@Req() req: Request) {
    console.log(req.headers);   // 요청 헤더
    console.log(req.query);     // 쿼리스트링
    console.log(req.ip);        // 클라이언트 IP
    return 'ok';
  }
}
```

## req 객체에서 꺼낼 수 있는 주요 값

| 속성 | 설명 |
|------|------|
| `req.body` | 요청 바디 (POST/PUT) |
| `req.params` | URL 경로 파라미터 |
| `req.query` | 쿼리스트링 파라미터 |
| `req.headers` | 요청 헤더 |
| `req.ip` | 클라이언트 IP 주소 |
| `req.user` | Passport.js가 붙여주는 인증된 사용자 정보 |

## @Req() vs 전용 데코레이터

- NestJS는 `@Req()` 대신 목적에 맞는 전용 데코레이터를 사용하는 것을 권장한다.
- 전용 데코레이터를 사용하면 코드가 간결해지고 테스트가 쉬워진다.

```ts
// @Req() 사용 — 권장하지 않음
@Get(':id')
find(@Req() req: Request) {
  return this.service.find(req.params.id, req.query.page);
}

// 전용 데코레이터 사용 — 권장
@Get(':id')
find(@Param('id') id: string, @Query('page') page: number) {
  return this.service.find(id, page);
}
```

## Passport.js와 함께 사용

- 소셜 로그인 콜백 등에서 `req.user`에 접근할 때 `@Req()`를 사용한다.

```ts
@Get('callback')
@UseGuards(AuthGuard('google'))
googleCallback(@Req() req: Request) {
  return req.user; // Passport가 주입한 사용자 정보
}
```
