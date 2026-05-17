- `useFactory`는 [[NestJS]] 모듈에서 프로바이더를 동적으로 생성할 때 사용하는 팩토리 함수 패턴이다.
- 일반 `useClass`/`useValue`와 달리 함수의 반환값이 프로바이더의 실제 인스턴스가 되며, [[비동기(asynchronous)]] 초기화도 지원한다.

## 기본 문법

```ts
{
  provide: TOKEN,
  useFactory: (dep1, dep2) => {
    return new MyService(dep1, dep2);
  },
  inject: [DEP1_TOKEN, DEP2_TOKEN], // 팩토리 함수에 주입할 의존성
}
```

- `inject` 배열에 나열한 토큰이 순서대로 팩토리 함수의 인자로 전달된다.

## 동기 팩토리 예시

```ts
// 환경 변수 기반으로 Redis 클라이언트를 동적 생성
import { ConfigService } from '@nestjs/config';
import Redis from 'ioredis';

@Module({
  providers: [
    {
      provide: 'REDIS_CLIENT',
      useFactory: (config: ConfigService) => {
        return new Redis({
          host: config.get('REDIS_HOST'),
          port: config.get<number>('REDIS_PORT'),
        });
      },
      inject: [ConfigService],
    },
  ],
  exports: ['REDIS_CLIENT'],
})
export class RedisModule {}
```

## 비동기 팩토리 예시

- 팩토리 함수가 `Promise`를 반환하면 [[NestJS]]가 resolve될 때까지 기다린다.

```ts
@Module({
  imports: [ConfigModule],
  providers: [
    {
      provide: 'DB_CONNECTION',
      useFactory: async (config: ConfigService) => {
        const uri = config.get<string>('MONGODB_URI');
        const connection = await mongoose.connect(uri); // 비동기 연결
        return connection;
      },
      inject: [ConfigService],
    },
  ],
})
export class DatabaseModule {}
```

## forRootAsync 패턴

- 공식 모듈(TypeORM, Mongoose 등)의 `forRootAsync()`는 내부적으로 `useFactory`를 활용한다.

```ts
MongooseModule.forRootAsync({
  imports: [ConfigModule],
  useFactory: (config: ConfigService) => ({
    uri: config.get<string>('MONGODB_URI'),
  }),
  inject: [ConfigService],
})
```

## useClass / useValue 와의 비교

| 방식 | 특징 |
|------|------|
| `useClass` | 클래스를 NestJS IoC 컨테이너가 자동 인스턴스화 |
| `useValue` | 이미 만들어진 값(객체, 상수)을 그대로 등록 |
| `useFactory` | 함수 실행 결과를 프로바이더로 등록, 비동기 지원, 의존성 주입 가능 |

## 활용 사례

- 환경 변수에 따라 다른 구현체를 선택해야 할 때
- DB 연결처럼 비동기 초기화가 필요할 때
- 외부 설정 값을 기반으로 클라이언트 객체를 생성할 때
