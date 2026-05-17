- [[NestJS]]에서 [[JWT(JSON Web Token)]]의 내용을 확인하는 [[JwtService]]의 [[메서드(Method)]]이다.
- [[jwtService.verifyAsync]]와 다르게 검증은 하지 않고 토큰의 내용만 확인할 수 있다.

- [[동기(Synchronous)]] [[메서드(Method)]]로 즉시 결과를 반환한다.
- 토큰의 유효성 검증 없이 내용만 확인할 수 있다.
- 잘못된 형식의 토큰인 경우 null을 반환한다.


## 문법

```ts
decode(token: string, options?: DecodeOptions): any
```
### secret

- 토큰 디코딩에 사용할 비밀키를 설정한다.
### complete

- true로 설정하면 헤더, 페이로드, 서명을 모두 반환한다.
### json

- true로 설정하면 결과를 JSON 객체로 반환한다.


## 예시

```ts
const decodedToken = this.jwtService.decode(token, { complete: true });

console.log(decodedToken);

// {
//   header: { alg: 'HS256', typ: 'JWT' },
//   payload: { username: 'tae', age: 25 },
//   signature: '...'
// }
```
