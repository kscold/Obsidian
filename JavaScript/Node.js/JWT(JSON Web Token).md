- JWT(JSON Web Token)는 당사자간에 정보를 [[JSON(Java Script Object Notation)]] 개체로 안전하게 전송하기 위한 컴팩트하고 독립적인 방식을 정의하는 개방형 표준이다.
- 이 정보는 디지털 서명이 되어 있으므로 확인하고 신뢰할 수 있다.

- 즉, 정보를 안전하게 저할 때 혹은 유저의 권한 같은 것을 체크를 하기 위해서 사용하는데 유용한 [[모듈(Module)]]이다.


## JWT의 구조

![[Pasted image 20240914212528.png]]


### Header

- 토큰에 대한 메타 데이터를 호함하고 있다.
- 타입, 해싱 알고리즘(SHA256, RSA 등

```json
{
	"alg": "HS256",
	"typ": "JWT",
}
```

### Payload

- 유저 정보(issuer), 만료 기간(expiration time), 주제(subject) 등
- iat 값은 토큰이 언제 만들어졌는지 알려 주는 값, exp 값은 언제 만료되는지 알려주는 값임

```json
{
	"sub": "1234567890",
	"name": "John Doe",
	"lat": 1516239022,
}
```

### Verify Signature

- JWT의 마지막 세그먼트는 토큰이 보낸 사람에 의해 서명되었으며 어떤 식으로 변경되지 않았는지 확인하는 데 사용되는 서명이다.
- 서명은 헤더 및 페이로드 세그먼트, 서명 알고리즘, 비밀 또는 공개 키를 사용하여 생성된다.

```json
HMACSHA256{
	base64UrlEncode(header) + "." +
	base64UrlEncode(payload),
	your-256-bit-secret	
}
```
