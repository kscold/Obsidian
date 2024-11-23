- 일반적으로 [[NestJS]]에서는 name.repository.ts으로 이름을 설정한다.

- [[@EntityRepository()]]


## [[TypeORM]] 2버전 이하 문법

- 생성 시 리파지터리 [[클래스(class)]]를 [[extends]] 해준다.

```ts
import { EntityRepository, Repository } from 'typeorm';  
import { Board } from './board.entity';  
  
@EntityRepository(Board)  
export class BoardRepository extends Repository<Board> {


}
```


## [[TypeORM]] 3버전 이상 문법

- 생성 시 리파지터리 [[@EntityRepository()]] [[데코레이터(Decorator)]] 대신 [[@Injectable()]] 데코레이터를 이용해여, [[생성자(Constructor)]]인 [[constructor()]]를 사용하여 Repository [[객체(Object)]]를 [[DataSource]]의 createEntityManager() 메서드를 이용하여 리파지터리를 선언한다.

```ts
import { Injectable } from '@nestjs/common';  
import { DataSource, Repository } from 'typeorm';  
import { Board } from './board.entity';  
  
@Injectable()  
export class BoardRepository extends Repository<Board> {  
    constructor(dataSource: DataSource) {  
        super(Board, dataSource.createEntityManager());  
    }  
}
```