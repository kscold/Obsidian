- Provider는 [[NestJS]]의 기본 개념으로 대부분의 기본 [[NestJS]] [[클래스(class)]]는 [[서비스(Service)]], [[리파지터리(Repository)]], 팩토리, 헬퍼 등 Provider로 취급될 수 있다.

- Provider의 주요 아이디어는 종속성으로 주입([[의존성 주입(Dependency Injection)]])할 수 있다는 것이다.
- 즉, [[NestJS]]는 providers에 등록되어 있는 파일들을 보고 [[의존성 주입(Dependency Injection)]] 관계를 확인한다.

- 따라서 [[객체(Object)]]는 서로 다양한 관계를 만들 수 있으며 [[객체(Object)]]의 [[인스턴스(Instance)]]를 "연결"하는 기능은 대부분 Nest [[런타임(runtime)]] 시스템에 위임될 수 있다.


## Provider에 등록

- Provider를 사용하기 위해서는 이것을 Nest에 등록해줘야지 사용할 수 있다.
- 등록하기 위해서는 module 파일에서 할 수 있다.

- module 팡리에 providers 항목안에 해당 모듈에서 사용하고자 하는 Provider를 넣어주면 된다.

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