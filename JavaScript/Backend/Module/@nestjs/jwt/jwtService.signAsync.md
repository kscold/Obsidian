- [[NestJS]]에서 [[JWT(JSON Web Token)]]를 생성하는 [[JwtService]]의 [[메서드(Method)]]이다.
- [[payload]]와 option을 받아 [[JWT(JSON Web Token)]]를 생성한다.


- [[Promise]]를 반환하는 [[비동기(asynchronous)]] [[메서드(Method)]]이다.
- [[JwtModule]]에서 설정한 기본 옵션을 [[재정의(Override)]]할 수 있다.


## 문법

```ts
signAsync(payload: any, options?: SignOptions): Promise<string>
```

## 예시

```ts
const token = await this.jwtService.signAsync(
    { username: 'tae', age: 25 },
    {
        secret: process.env.JWT_SECRET,
        expiresIn: '1h'
    }
);
```
### secret

- 토큰 생성에 사용할 비밀키를 설정한다.
### expiresIn

- 토큰의 만료 시간을 설정한다.
### algorithm

- 암호화 알고리즘을 설정한다.