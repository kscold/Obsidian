- [[NestJS 모듈(module)]]을 설정하는 [[정적 메서드(Static method)]]이다.
- 각 모듈별로 독립적인 설정을 적용할 때 사용한다.

- [[module.forRoot()]]와 달리 각 [[NestJS 모듈(module)]]마다 다른 설정을 적용할 수 있다.
- 동일한 모듈을 여러 번 [[import]]하면서 각각 다른 설정을 적용할 수 있다.


## 예시

```ts
@Module({
    imports: [
        JwtModule.register({
            secret: 'your-secret-key',
            signOptions: { expiresIn: '1h' },
        }),
    ],
})
```

- [[동기(Synchronous)]]적으로 설정을 적용한다.
- 설정값이 정적이며 즉시 사용 가능할 때 사용한다.
- [[NestJS 모듈(module)]]별로 독립적인 설정이 필요할 때 적합하다.