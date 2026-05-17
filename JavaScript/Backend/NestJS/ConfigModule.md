- ConfigModule은 [[NestJS]]에서 환경 변수를 로드하고 애플리케이션 전역에서 사용할 수 있게 해주는 공식 모듈이다.
- `@nestjs/config` 패키지에 포함되어 있으며, 내부적으로 `dotenv`를 사용해 `.env` 파일을 파싱한다.

## 설치

```bash
pnpm add @nestjs/config
```

## 기본 설정

- `AppModule`에서 `ConfigModule.forRoot()`를 호출해 모듈을 등록한다.
- `isGlobal: true` 옵션을 주면 다른 모듈에서 별도 import 없이 `ConfigService`를 주입받을 수 있다.

```ts
// app.module.ts
import { Module } from '@nestjs/common';
import { ConfigModule } from '@nestjs/config';

@Module({
  imports: [
    ConfigModule.forRoot({
      isGlobal: true,   // 전역 모듈로 등록
      envFilePath: '.env', // 로드할 .env 파일 경로 (기본값)
    }),
  ],
})
export class AppModule {}
```

## ConfigService로 환경 변수 읽기

- `ConfigService`를 주입받아 `get()` 메서드로 [[환경 변수]] 값을 읽는다.

```ts
import { Injectable } from '@nestjs/common';
import { ConfigService } from '@nestjs/config';

@Injectable()
export class DatabaseService {
  constructor(private readonly configService: ConfigService) {}

  getDbUri(): string {
    return this.configService.get<string>('MONGODB_URI'); // .env의 MONGODB_URI 값 반환
  }

  getPort(): number {
    return this.configService.get<number>('PORT', 3000); // 없으면 기본값 3000
  }
}
```

## 커스텀 설정 파일 분리

- `load` 옵션으로 설정 팩토리 함수를 분리할 수 있다.

```ts
// config/database.config.ts
export default () => ({
  database: {
    uri: process.env.MONGODB_URI,
    name: process.env.DB_NAME ?? 'dev',
  },
});
```

```ts
// app.module.ts
import databaseConfig from './config/database.config';

ConfigModule.forRoot({
  isGlobal: true,
  load: [databaseConfig],
})
```

```ts
// 읽을 때 네임스페이스로 접근
this.configService.get('database.uri');
```

## 환경별 .env 파일 분리

```ts
ConfigModule.forRoot({
  envFilePath: process.env.NODE_ENV === 'production' ? '.env.prod' : '.env.dev',
})
```

## 주의사항

- `ConfigModule`은 반드시 다른 모듈보다 먼저 로드되어야 한다.
- [[환경 변수]] 값은 항상 `string` 타입으로 반환되므로, 숫자 등 다른 타입은 직접 변환이 필요하다.
- 민감한 시크릿은 `.env` 파일에 두되 절대 git에 커밋하지 않는다.
