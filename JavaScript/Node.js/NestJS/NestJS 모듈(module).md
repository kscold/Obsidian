- Module이란, [[@Module()]] [[데코레이터(Decorator)]]가 붙어있는 [[클래스(class)]]를 의미한다.
- [[@Module()]] [[데코레이터(Decorator)]]는 [[NestJS]]가 전체 어플리케이션의 구조를 만들어나가는데 사용하기 위한 [[메타데이터(Metadata)]])를 제공한다.

- [[NestJS]] 의 프로젝트에는 최소 한 개 이상의 [[모듈(Module)]]이 존재하며, 이때 최초 프로젝트 생성 시에 만들어지는 최소 한 개의 [[모듈(Module)]]은 Root Module이다.
- 즉, 프로젝트 내에 Root Module만 존재하는 태초의 상태이다.

- 모듈은 기본적으로 [[프로바이더(Provider)]]들을 캡슐화하고 있다.
- 이 말은 즉, 사용하려고 하는(또는 [[의존성 주입(Dependency Injection)]]되는) [[프로바이더(Provider)]] 가 해당 모듈 안에 직접적으로 속해있지 않거나, 또는 특정 모듈에서 다른 외부 모듈에 속한 [[프로바이더(Provider)]] 를 필요로 할 때 [[export]] 속성에 의해 지정되지 않은 [[프로바이더(Provider)]] 는 사용하지 못 한다는 것이다. 
- 따라서, 어떤 [[프로바이더(Provider)]]를 [[export]] 한다는 것은 어플리케이션의 다른 [[모듈(Module)]]에서 사용할 수 있는 [[public]] [[Interface]] 또는 API 로 허용한다는 것을 의미한다. 

- 자바로 정의하면, 말 그대로 [[접근 제어자(Access modifier)]]를 [[private]]에서 [[public]] 으로 변경하는 것과 같은 효과이다.


## NestJS의 [[모듈(Module)]] 구조

- Root Module 은 [[NestJS]] 가 Application Graph를 구축하는데 필요한 시작점이다.
- 또한, [[NestJS]]가 Module과 [[프로바이더(Provider)]]간의 관계, 그리고 종속성 관리를 위해 사용하는 내부적인 데이터 구조이다.

![](https://velog.velcdn.com/images/dinb1242/post/c500f78d-b3a9-42a7-94da-ae0bb076d295/image.png)

- 단 하나의 Root Module 만 갖는 매우 간단한 형태의 어플리케이션은 보기 힘들 것이다.
- 실제 배포된 어플리케이션은 다수의 모듈을 가지고 있을 것이며, 각각의 모듈은 밀접하게 관련있는 기능(인증, 게시판 등)들을 캡슐화하는 형태일 것이다.


## NestJS의 [[모듈(Module)]]사용 패턴

- 아래 코드처럼 Root Module을 정의한다.

```ts
@Module({
	imports: [],
	controllers: [],
	providers: [],
})

export class AppModule {}
```

```ts
// common.module.ts
@Module({
	controllers: [],
	imports: [
	    ConfigModule.forRoot({
		    load: configs,
		    isGlobal: true,
	    }),
		MysqlModule,
	    RedisModule,
	],
})
export class CommonModule {}
```

```ts
@Module({
	imports:[
	    TypeOrmModule.forFeature([
		    Alarms, // entitie
		    ClientAlarms, // entitie
		    Counseling // entitie
		]),
	],
	providers: [AlarmsService, AlarmsBuilder],
	controllers: [AlarmsController]
})
export class AlarmsModule {}
```

### [[forRoot()]]

- 동적 모듈을 한번 구성하고 해당 구성을 여러 위치에서 재사용한다.

- 위의 두번째 코드 블럭에서 [[ConfigModule]]은 common.module.ts 파일에서 한번 구성하고, 다른 파일에서 [[import]]해서 사용한다.

```ts
import { Module } from '@nestjs/common';
import { ConfigModule, ConfigService } from '@nestjs/config';
import { TypeOrmModule, TypeOrmModuleOptions } from '@nestjs/typeorm';

@Module({
	imports: [
	    TypeOrmModule.forRootAsync({
		    imports: [ConfigModule],
		    inject: [ConfigService],
		    useFactory: async (configService: ConfigService) => {
		        return {
			        type: 'mysql',
					...
			    } as TypeOrmModuleOptions;
		    },
	    }),
	],
})

export class MysqlModule {}
```

### [[forFeature()]]

- 루트용 동적 모듈의 구성을 사용할 예정이지만 호출 모듈의 필요에 따라 일부 구성을 수정해야 한다.  
- 자체 주입 토큰이 있는 동적 공급자를 생성하기 위해 사용한다.  
  
- 예를 들어 TypeOrmModule을 살펴보자.

- Mysql에서 TypeOrmModule.forRootAsync()로 [[데이터베이스(DataBase)]]를 연동하여 커넥션을 생성한 이후 코드이다.

```ts
// common.module.ts
import { Module } from '@nestjs/common';
import { ConfigModule, ConfigService } from '@nestjs/config';
import { TypeOrmModule, TypeOrmModuleOptions } from '@nestjs/typeorm';

@Module({
	imports: [
	    TypeOrmModule.forRootAsync({
		    imports: [ConfigModule],
		    inject: [ConfigService],
		    useFactory: async (configService: ConfigService) => {
		        return {
					...
			    } as TypeOrmModuleOptions;
		    },
		}),
	],
})

export class MysqlModule {}
```

- 각 [[모듈(Module)]]에 사용하는 [[엔티티(Entity)]]들을 지정해주어야 한다.  

- boards [[모듈(Module)]]에서 Board라는 [[엔티티(Entity)]]를 사용하겠다는 의미이다.  
- 그리고 이 코드 덕분에 boards.service에 주입할 수 있다.

```ts
// boards.module.ts
import { Module } from '@nestjs/common';
import { TypeOrmModule } from '@nestjs/typeorm';
import { Board } from './entities/board.entities';
import { BoardsController } from './boards.controller';
import { BoardsService } from './boards.service';

@Module({
	imports: [TypeOrmModule.forFeature([Board])],
	controllers: [BoardsController],
	providers: [BoardsService],
})

export class BoardsModule {}
```

- boards.service.ts 파일에서 Board [[엔티티(Entity)]]를 사용할 수 있다.

```ts
// boards.service.ts
import { Injectable, NotFoundException } from '@nestjs/common';
import { InjectRepository } from '@nestjs/typeorm';
import { Repository } from 'typeorm';
import { UpdateBoardDto } from './dto/update-board.dto';
import { CreateBoardDto } from './dto/create-board.dto';
import { Board } from './entities/board.entities';
import { BoardStatus } from './board-status.enum';

@Injectable()
export class BoardsService {
	constructor(
	    @InjectRepository(Board) private boardRepository: Repository<Board>,
	) {}
	
	async getBoards(): Promise<Board[]> {
	    return this.boardRepository.find();
	}
	
	...
}
```