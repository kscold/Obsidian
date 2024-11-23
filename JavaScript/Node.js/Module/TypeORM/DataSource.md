- 개발 환경 내에서 [[데이터베이스(DataBase)]]와 상호작용 하기 위해서는, Datasource를 먼저 설정해야 한다. 
- [[TypeORM]]의 DataSource는 DB connection 설정을 유지하고, 사용하고 있는 RDBMS에 의지하여 connection pool 또는 초기 db 연결 상태를 초기 db connection을 구축한다.  

- 초기 connection 또는 connection pool을 구축하기 위해서는, DataSrouce [[객체(Object)]]의 initialize 메서드를 호출해야 한다.
- 그러나 [[의존성 주입(Dependency Injection)]]을 받을 필요는 없다.
- 연결 해제는 destroy 메서드로 실행한다.  
- 일반적으로, 어플리케이션 부트스트랩에서 Datasource의 initialize 메서드를 호출하고, [[데이터베이스(DataBase)]]와 관련한 일을 모두 완료한 후에 destroy 한다.

- 그러나 실제로는, 백엔드 [[서버(Server)]]는 실행을 계속 유지하므로, DataSource를 destroy하는 경우는 존재하지 않는다.  


## DataSource 전역 변수로 생성 후 설정(setup)

- new DataSource를 호출하여 [[생성자(Constructor)]]를 실행시킴으로써 [[초기화(initialization)]]해야 한다. 
- 전역 [[변수(Variable)]]로 설정하도록 해야 한다.

- AppDataSource라는 이름으로 전역 DataSource [[변수(Variable)]]를 생성하여, 즉 전역으로 [[export]]t할 수 있도록 만들어 어플리케이션 사이에서 이 [[인스턴스(Instance)]]를 사용할 수 있게 된다. 

```ts
import { DataSource } from "typeorm"

const AppDataSource = new DataSource({
	type: "mysql",
	host: "localhost",
	port: 3306,
	username: "test",
	password: "test",
	database: "test",
})

AppDataSource.initialize()
	.then(() => {
		console.log("Data Source has been initialized!")
	})
	.catch((err) => {
		console.error("Error during Data Source initialization", err)
	})
```


### DataSource 사용하기

- DataSource 정의를 한 후에, 앱 어디에서든 다음과 같이 사용할 수 있다.  
- DataSource [[객체(Object)]]를 사용하기 위해, manager 또는 getRepository() 옵션을 사용할 수 있는데 아래는 manager 옵션의 find() 메서드를 통해 모든 유저 정보를 가져오도록 하는 예시로 작성되었다.

```ts
import { AppDataSource } from "./app-data-source"
import { User } from "../entity/User"

export class UserController {
    @Get("/users")
    getAll() {
        return AppDataSource.manager.find(User)
    }
}
```

## [[NestJS]] DataSource 설정

- [[NestJS]]의 경우 TypeOrmModuleOptions를 이용한다.

```ts
import { TypeOrmModuleOptions } from '@nestjs/typeorm';  
  
export const typeORMConfing: TypeOrmModuleOptions = {  
    // Database Type 
    type: 'postgres',  
    host: 'localhost',  
    port: 5432,  
    username: 'postgres',  
    password: 'postgres',  
    database: 'board-app',  
    entities: [__dirname + '/../**/*.entity.{js,ts}'],  
    synchronize: true,  
};
```