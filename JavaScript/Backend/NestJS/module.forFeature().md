- 이 [[메서드(Method)]]는 특정 [[NestJS 모듈(module)]]에서 사용된다.
- 루트용 동적 [[모듈(Module)]]의 구성을 사용할 예정이지만 호출 [[모듈(Module)]]의 필요에 따라 일부 구성을 수정해야한다.  
- 자체 주입 토큰이 있는 동적 공급자([[Providers]])를 생성하기 위해 사용한다.


## [[리포지토리(Repository)]]의 주입

- [[데이터베이스(DataBase)]] 테이블과 직접적으로 관련된 [[리포지토리(Repository)]]를 등록하기 위해 사용된다.
- 여기서 [[리포지토리(Repository)]]는 [[TypeORM]]의 패턴으로, [[데이터베이스(DataBase)]] [[테이블(Table)]]에 대한 모든 작업(조회, 삽입, 업데이트, 삭제 등)을 캡슐화한다.

- forFeature() [[메서드(Method)]]는 특정 [[NestJS 모듈(module)]]에서만 사용할 [[리포지토리(Repository)]]를 등록하므로, [[모듈(Module)]]의 의존성을 명확히 관리할 수 있게 도와준다.
- 이를 통해 각 [[NestJS 모듈(module)]]은 필요한 [[리포지토리(Repository)]]만을 로드하여 메모리 사용을 최적화하고, 애플리케이션의 구조를 깔끔하게 유지할 수 있다.


## 예시

- 예를 들어 TypeOrmModule을 살펴보자.
- Mysql에서 TypeOrmModule.forRootAsync()로 DB 연동하여 커넥션을 생성한 이후 코드이다.

```ts
// src/common/common.module.ts
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

- 각 모듈에 사용하는 [[엔티티(Entity)]]들을 지정해주어야 한다.  

- boards 모듈에서 Board라는 엔터티를 사용하겠다는 의미이다.  
- 그리고 이 코드 덕분에 boards.service에 주입할 수 있다.

```ts
// src/modules/v1/boards/boards.module.ts
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
// src/modules/v1/boards/boards.service.ts
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