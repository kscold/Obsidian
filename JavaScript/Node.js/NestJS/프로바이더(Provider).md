- 프로바이더(Provider)는 [[NestJS]]의 기본 개념으로 대부분의 기본 [[NestJS]] [[클래스(class)]]는 [[서비스(Service)]], [[리포지토리(Repository)]], 팩토리, 헬퍼 등 Provider로 취급될 수 있다.

- Provider의 주요 아이디어는 종속성으로 주입([[의존성 주입(Dependency Injection)]])할 수 있다는 것이다.
- 즉, [[NestJS]]는 providers에 등록되어 있는 파일들을 보고 [[의존성 주입(Dependency Injection)]] 관계를 확인한다.

- 따라서 [[객체(Object)]]는 서로 다양한 관계를 만들 수 있으며 [[객체(Object)]]의 [[인스턴스(Instance)]]를 "연결"하는 기능은 대부분 Nest [[런타임(runtime)]] 시스템에 위임될 수 있다.


## 프로바이더(Provider)에 등록

- 프로바이더(Provider)를 사용하기 위해서는 이것을 [[NestJS]]에 등록해줘야지 사용할 수 있다.
- 등록하기 위해서는 [[NestJS 모듈(module)]] 파일에서 할 수 있다.

- module 파일에 providers 항목안에 해당 모듈에서 사용하고자 하는 프로바이더(Provider)를 넣어주면 된다.

- 사실 [[NestJS 모듈(module)]]의 providers 속성의 원형은 아래 코드와 같다.
- provide의 [[속성(Property)]]의 경우는 고유한 [[key]]를 넣어주어야하나, 고유한 [[key]]를 넣어주지 않아도 [[NestJS]]가 내부적으로 [[클래스(class)]]의 이름을 사용하여 고유한 [[key]]로 사용한다.

- 또한 useClass뿐만아니라 useValue나 [[useFactory]]도 사용 가능하다.

```js
import { Module } from '@nest/common';
import { BoardsController } from './boards.controller';
import { BoardsService } from './boards.service';

@Module({
	controllers: [BoardsController],
	providers: [
		{  
		    provide: BoardsService, // NestJS가 고유한 key 사용
		    // provide: "CUSTOM_KEY", // 개발자가 고유한 key 사용
		    useClass: BoardsService,  
		    // useValue: 123,  // 특별한 값들도 의존성 주입 가능
			// useFactory: () = > { // useFactory 사용하여 함수도 가능
			// ...
			//    return {
			//		  a: "b"
			//    }
			// }
		},
	]
})

export class BardsModule { }
```

- 그러나 [[NestJS]]에서는 간단하게 [[@Injectable()]] [[데코레이터(Decorator)]]로 [[의존성 주입(Dependency Injection)]]을 받는 [[함수(Function)]]만 선언해도 된다.

```js
import { Module } from '@nest/common';
import { BoardsController } from './boards.controller';
import { BoardsService } from './boards.service';

@Module({
	controllers: [BoardsController],
	providers: [BoardsService]
})

export class BardsModule { }
```



