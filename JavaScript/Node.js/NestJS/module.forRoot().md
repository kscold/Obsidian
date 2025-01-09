- [[NestJS]]의 forRoot() [[메서드(Method)]]는 애플리케이션의 루트 [[NestJS 모듈(module)]]에서 사용된다.
- 즉, 동적 [[모듈(Module)]]을 한번 구성하고 해당 구성을 여러 위치에서 재사용하기 위해 사용된다.

- 주로 [[데이터베이스(DataBase)]] 연결 설정(데이터베이스 유형, 호스트, 포트, 사용자 이름, 비밀번호 등)을 전역적으로 초기화하는 데 사용된다.
- 이는 데이터베이스와의 연결을 설정하고 전체 애플리케이션에서 해당 연결을 재사용할 수 있도록 한다.


## 예시

 아래 코드처럼 루트 모듈을 정의한다.

```ts
@Module({
	imports: [],
	controllers: [],
	providers: [],
})

export class AppModule {}
```

```ts
// src/common/common.module.ts
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

- 두번째 코드 블럭에서 ConfigModule은 Common.module.ts 파일에서 한번 구성하고, 다른 파일에서 import해서 사용한다.

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
