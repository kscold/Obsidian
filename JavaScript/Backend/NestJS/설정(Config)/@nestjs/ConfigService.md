- [[NestJS]]에서 [[환경 변수]]와 구성 값을 관리하는 [[서비스(Service)]]이다.
- .env 파일이나 다른 구성 소스에서 값을 가져와 사용할 수 있게 해준다.

- ConfigServices는 타입 안전성, 기본값 설정 가능, 중첩된 구성 객체를 지원하는 등의 장점이 있다.
- 또한 [[process]].env를 통해 직접 접근하는 것보다 안전하고 체계적인 방법을 제공하며 [[의존성 주입(Dependency Injection)]]을 통해 쉽게 사용할 수 있다.


## 기본 사용법

```ts
import { ConfigService } from '@nestjs/config';

@Injectable()
export class AppService {
    constructor(private configService: ConfigService) {}
	
    getData() {
        // 환경 변수 가져오기
        const dbHost = this.configService.get<string>('DATABASE_HOST');
        const port = this.configService.get<number>('PORT', 3000);
    }
}
```


## 메서드

### configService.get()

- 설정된 값을 가져오는 메서드이다.
### configService.has()

- 특정 키가 존재하는지 확인하는 메서드이다.
### configService.getOrThrow()

- 값이 없으면 [[에러(error)]]를 발생시키는 메서드이다.


