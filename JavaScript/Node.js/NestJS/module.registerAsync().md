- [[NestJS 모듈(module)]]을 [[비동기(asynchronous)]]적으로 설정하는 [[정적 메서드(Static method)]]이다.

- 설정값을 [[비동기(asynchronous)]]적으로 불러와야 할 때 사용한다.
- 즉, [[Promise]]를 반환하는 [[비동기(asynchronous)]] 설정이 가능하다.

- 다른 [[서비스(Service)]]나 [[NestJS 모듈(module)]]에 [[의존성 주입(Dependency Injection)]]을 할 수 있다.
- [[런타임(runtime)]]에 동적으로 설정값을 결정할 수 있다.


## 예시

```ts
@Module({
    imports: [
        JwtModule.registerAsync({
            imports: [ConfigModule],
            useFactory: async (configService: ConfigService) => ({
                secret: await configService.get('JWT_SECRET'),
                signOptions: { 
                    expiresIn: configService.get('JWT_EXPIRES_IN') 
                },
            }),
            inject: [ConfigService],
        }),
    ],
})
```

- [[ConfigService]]나 다른 [[의존성 주입(Dependency Injection)]]이 필요한 경우에 사용한다.
- [[useFactory]] 패턴을 통해 [[비동기(asynchronous)]]적으로 설정을 구성할 수 있다.