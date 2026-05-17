- `@IsEmail()`은 class-validator에서 문자열이 유효한 이메일 주소 형식인지 검사하는 [[데코레이터(Decorator)]]이다.
- 내부적으로 `validator.js`의 `isEmail()` 함수를 사용하며, RFC 5322 기준의 이메일 형식을 검증한다.

## 설치

```bash
pnpm add class-validator class-transformer
```

## 기본 사용법

```ts
import { IsEmail, IsNotEmpty } from 'class-validator';

class LoginDto {
  @IsEmail({}, { message: '올바른 이메일 형식이 아닙니다.' })
  @IsNotEmpty({ message: '이메일을 입력해주세요.' })
  email: string;
}
```

## 유효/무효 이메일 예시

```ts
// 유효한 이메일
'user@example.com'      // ✓
'user+tag@gmail.com'    // ✓
'user@sub.domain.co.kr' // ✓

// 무효한 이메일
'user@'                 // ✗ (도메인 없음)
'@example.com'          // ✗ (로컬 파트 없음)
'user.example.com'      // ✗ (@ 없음)
'user @example.com'     // ✗ (공백 포함)
```

## NestJS ValidationPipe와 함께 사용

- `ValidationPipe`를 전역에 등록하면 DTO에 선언한 `@IsEmail()` 규칙이 자동으로 적용된다.

```ts
// main.ts
app.useGlobalPipes(new ValidationPipe({ whitelist: true }));
```

```ts
// auth.controller.ts
@Post('login')
async login(@Body() dto: LoginDto) {
  return this.authService.login(dto);
  // email이 잘못된 형식이면 400 BadRequest 자동 반환
}
```

## 옵션 파라미터

```ts
@IsEmail(
  { allow_display_name: true }, // "Alice <alice@example.com>" 형식 허용
  { message: '이메일 형식을 확인하세요.' }
)
email: string;
```

| 옵션 | 기본값 | 설명 |
|------|--------|------|
| `allow_display_name` | false | "Name <email>" 형식 허용 |
| `require_tld` | true | TLD(.com, .kr 등) 필수 여부 |
| `allow_ip_domain` | false | IP 주소 도메인 허용 |

## 관련 데코레이터

- `@IsNotEmpty()` — 빈 문자열 불허
- `@IsString()` — 문자열 타입 검사
- `@MaxLength()` — 최대 길이 제한 (이메일 필드는 보통 255자 제한)
