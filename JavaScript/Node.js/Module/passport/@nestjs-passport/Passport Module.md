- [[NestJS]]에서 [[passport]]을 사용하기 위해서는 Passport 모듈을 생성해주면 된다.

- 필요한 [[NestJS 모듈(module)]]에서 [[passport]]를 등록해주면 되지만 그렇게 되면 위에서 구현한 JwtStrategy 또한 [[Providers]]로 등록을 해야한다.


- 원하는 것은 다른 `configModule`들 처럼 `app.module`에서 설정을 포함한 Passport 모듈을 import하고, 사용하는 `module`에서 Passport 모듈만 import하여 쓰기를 원하기 때문에 아래와 같이 작성하였다.

```ts
@Module({
	imports: [
		// PassportModule을 통해 jwt를 등록
		PassportModule.register({ defaultStrategy: 'jwt' }),
		JwtModule.register({  
		    secret: 'Secret1234', // 토큰을 생성할 때 사용하는 Secret Key임
		    signOptions: {  
	        expiresIn: 60 * 60,  
	    })
	], 
	providers: [JwtStrategy],
	exports: [PassportModule],
})
export class PassportConfigModule {}
```

- [[passport]] [[모듈(Module)]]을 import 한다. 
- 만약 기본 전략을 설정하고 싶다면 defaultStrategy를 설정하면 된다.
- provider로 구현한 전략을 추가해준다.