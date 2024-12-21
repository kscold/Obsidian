- [[NestJS]]에서 [[passport]]을 사용하기 위해서는 [[passport]] [[NestJS 모듈(module)]]을 생성해주면 된다.

- 필요한 [[NestJS 모듈(module)]]에서 [[passport]]를 등록해주면 되지만 그렇게 되면 위에서 구현한 JwtStrategy 또한 [[Providers]]로 등록을 해야한다.


## Passport 특징

### 모듈화된 인증 시스템 

- 다양한 전략(Strategy)를 쉽게 연결해서 사용 가능하다.
- 인증관련 작성할 코드가 많이 줄어든다.
### [[NestJS 미들웨어(Middleware)]] 기반 디자인

- 요청, 응답, [[NestJS 생명 주기(LifeCycle)]]에 비파괴적 방식으로 통합된다.
### 일반화된 가벼운 코어

- [[passport]] 코어는 넓은 전략을 수용할 수 있도록 가볍고 일반적(Unopinonated)으로 설계되었다.
### [[세션(Session)]] 및 토큰 방식 사용

- [[세션(Session)]] 기반과 토큰 기반의 인증 시스템 모두 사용 가능하다.
### 방대한 생태계

- 다양한 오픈소스 전략들이 무료로 공개되어있다.
- 어려운 부붑은 직접 코딩할 필요 없을 가능성이 높다.


## Passport 사용방법

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

### Strategy 구현

- 아래와 같이 특정 Strategy를 [[extends]]하면 되어 어떤 Strategy를 [[상속(Inheritance)]]받냐에 따라서 validate() [[메서드(Method)]]의 구현이 달라진다.

```ts
@Injectable()
export class LocalStrategy extends PassportStrategy(Strategy){
	constructor(private authService: AuthService) {
		super();
	}
	
	async validate(){
		// 구현
	}
}
```

### Passport 적용

- 적용은 [[@UseGuards()]] [[데코레이터(Decorator)]]를 사용하여 [[AuthGuard()]]를 적용시키면 된다.

```ts
@Controller('auth')
export class AuthController {
	@UseGuards(AuthGuard('loacl'))
	@post('login')
	async login(@Request() req) {
		// 구현
	}
}
```