- [[NestJS]]에서 `@nestjs/jwt` [[JWT(JSON Web Token)]] 토큰을 검증하는 [[JwtService]]의 [[메서드(Method)]]이다

- option에 비밀번호 설정, 만료기한 등을 위의 [[NestJS 모듈(module)]]에서 설정할 수 있다.
- 만료기한이 다르거나 [[JWT(JSON Web Token)]] 비밀번호 값을 다르게 설정하려면 이 option 안에 설정하여 각 특징이 다른 [[JWT(JSON Web Token)]]를 발행하여 사용할 수 있다.


## 문법

```ts
signAsync(token에 넣을 정보,{option})  
```


## JWT 검증하기

```ts
const payload = await this.jwtService.verifyAsync(token,
	{
		secret: process.env.JWT_SECRET
	})
 
 console.log(payload)
 
 // {username:'tae', age:25, address:'seoul'}
```

- 검증이 성공하면 [[payload]] 데이터를 반환한다.
- 검증이 실패하면 [[에러(error)]]를 발생시킨다.



