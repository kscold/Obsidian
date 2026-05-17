- [[NestJS]]에서 [[JWT(JSON Web Token)]]를 다루기 위한 서비스이다

- [[JWT(JSON Web Token)]] 토큰의 생성, 검증 등의 기능을 제공한다


## 문법

- [[JwtModule]]에서 secret, expiresIn 등의 옵션을 설정할 수 있다.

- 각 [[메서드(Method)]]드 호출 시에도 개별적인 옵션 설정이 가능하다.

```ts
@Injectable()
export class AuthService {
	constructor(private jwtService: JwtService) {}
	
	// JWT 토큰 생성
	async createToken(payload: any) {
	    return await this.jwtService.signAsync(payload)
	}
	
	// JWT 토큰 검증
	async verifyToken(token: string) {
	    return await this.jwtService.verifyAsync(token)
	}
}
```


## [[메서드(Method)]]
### [[jwtService.verifyAsync]]

- JWT 토큰을 생성하는 메서드이다
### [[jwtService.verifyAsync]]

- JWT 토큰을 검증하는 메서드이다.