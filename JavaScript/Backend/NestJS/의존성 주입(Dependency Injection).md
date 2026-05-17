- 의존성 주입은 제어의 역전(Inversion of Control) 기술 중 하나이다.

- 의존성 주입은 개발자가 필요한 외부 자원([[클래스(class)]], [[함수(Function)]] 등)을 제공받을 수 있도록 하는 것이다.
- 즉, 쉽게 말해 [[서비스(Service)]] 파일 내의 [[함수(Function)]]를 컨트롤러에서 사용할 수 있도록 하는 것이다.

## 제어의 역전

- 개발자가 제어할 영역을 프레임워크에 넘기는 것이다.


## Nest의 의존성 주입

- Nest의 의존성 주입은 [[클래스(class)]]의 [[생성자(Constructor)]] [[constructor()]] 안에서 이루어진다.

- 아래와 같이 선언하면 boardsService를 다른 메서드에서도 사용할 수 있다.

```ts
import { Controller } from '@nestjs/common';  
import { BoardsService } from './boards.service';  
  
@Controller('boards')  
export class BoardsController {  
	// 3. 다른 메서드에서 사용할 수 있도록 property를 선언
	boardsService: BoardsService;  
	  
	// 1. 생성자 parameter 주입  
	constructor(boardsService: BoardsService) {  
	    this.boardsService = boardsService; // 2. parameter를 property화  
	}  
}
```

- [[타입스크립트(TypeScript)]]의 [[private]]를 사용하면 위의 과정을 생략하여 아래코드처럼 사용할 수 있다.

```ts
import { Controller } from '@nestjs/common';  
import { BoardsService } from './boards.service';  
  
@Controller('boards')  
export class BoardsController {  
	constructor(private boardsService: BoardsService) {}
}
```

## Provider 타입

- NestJS의 `@Module()` providers 배열에는 단순 클래스 외에도 여러 방식으로 의존성을 등록할 수 있다.

| 타입 | 용도 |
|------|------|
| `useClass` | 클래스를 직접 등록 (기본값) |
| `useValue` | 상수, 설정값, Mock 객체 등록 |
| `useFactory` | 팩토리 함수로 동적으로 인스턴스 생성 |
| `useExisting` | 기존 프로바이더의 별칭(alias) 생성 |

```ts
@Module({
  providers: [
    // useClass (기본)
    UserService,

    // useValue — 설정 객체나 Mock 주입
    { provide: 'CONFIG', useValue: { apiUrl: 'https://api.example.com' } },

    // useFactory — 비동기 설정, 조건부 생성
    {
      provide: 'DB_CONNECTION',
      useFactory: async (config: ConfigService) => {
        return await createConnection(config.get('DATABASE_URL'));
      },
      inject: [ConfigService],
    },

    // useExisting — 별칭
    { provide: 'AliasService', useExisting: UserService },
  ],
})
export class AppModule {}
```

- [[프로바이더(Provider)]]는 `provide` 키로 토큰을 지정하고, 해당 토큰으로 `@Inject()`하여 주입받는다.
- [[useFactory]]는 `inject` 배열에 의존성을 명시하면 팩토리 함수의 인자로 순서대로 전달된다.
- [[ConfigModule]]과 함께 `useFactory`를 조합하면 환경변수 기반 동적 설정 주입이 가능하다.
```