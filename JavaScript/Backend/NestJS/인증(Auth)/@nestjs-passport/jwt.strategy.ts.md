- [[NestJS]]에서 [[passport]]를 활용한 [[JWT(JSON Web Token)]] 인증을 구현할 때에는 @nestjs/passport([[Passport Module]]), [[passport-jwt]]를 사용한다.


## 구현

- 아래 코드는 jwt.strategy.ts의 구현이다.

```ts
import { Injectable, UnauthorizedException } from '@nestjs/common';  
import { PassportStrategy } from '@nestjs/passport';  
import { ExtractJwt, Strategy } from 'passport-jwt';  
import { InjectRepository } from '@nestjs/typeorm';  
import { UserRepository } from './user.repository';  
import { User } from './user.entity'; // 전략 사용은 일반 passport가 아니라 passport-jwt로 사용  
  
@Injectable() // 다른곳에서도 주입을 하기 위해 사용  
export class JwtStrategy extends PassportStrategy(Strategy) {  
    constructor(  
        @InjectRepository(UserRepository)  
        private userRepository: UserRepository,  
    ) {  
        super({  
            secretOrKey: 'Secret1234', // 토큰을 유효한지 체크할 때 사용하는 Secret Key임  
            jwtFromRequest: ExtractJwt.fromAuthHeaderAsBearerToken(), 
            // jwt를 Header에서 Bearer를 통해 가져오는 것으로 확인  
        });  
    }  
	  
    async validate(payload) {  
        const { username } = payload;  
        const user: User = await this.userRepository.findOne({ where: { username } });  
		  
        if (!User) {  
            throw new UnauthorizedException();  
        }  
		
		return user;
    }  
}
```

- [[constructor()]] [[메서드(Method)]]안의 [[super()]]를 통해 토큰이 유효한지 체크가 되면 validate [[메서드(Method)]]에서 payload에 있는 유저이름이 [[데이터베이스(DataBase)]]에서 있는 유저인지 확인 후 유저 [[객체(Object)]]를 반환한다.
- 이 후 return 값은 [[@UseGuards()]]를 이용한 모든 요천의 Request [[객체(Object)]]에 들어간다.