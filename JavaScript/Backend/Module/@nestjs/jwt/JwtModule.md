- [[NestJS]]에서 [[JWT(JSON Web Token)]]를 사용하기 위한 [[NestJS 모듈(module)]]이다.
- [[JWT(JSON Web Token)]] 관련 기능을 제공하는 [[JwtService]]를 주입받아 사용할 수 있다.


## 문법

```ts
import { JwtModule } from '@nestjs/jwt';

@Module({
    imports: [
        JwtModule.register({
            secret: process.env.JWT_SECRET,
            signOptions: {
                expiresIn: '1h'
            },
        }),
    ],
})
```

### secret

- [[JWT(JSON Web Token)]] 토큰을 생성하고 검증할 때 사용하는 비밀키를 설정한다.
### signOptions

- [[JWT(JSON Web Token)]] 토큰 생성 시 사용할 옵션들을 설정한다.
### expiresIn

- 토큰의 만료 시간('1h', '7d', '60s')을 설정한다.


## 주요 기능

- [[JwtService]]를 통해 토큰의 생성([[jwtService.signAsync]])과 검증([[jwtService.verifyAsync]])이 가능하다.
