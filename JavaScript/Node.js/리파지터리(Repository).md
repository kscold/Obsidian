- 일반적으로 [[NestJS]]에서는 name.repository.ts으로 이름을 설정한다.

- [[@EntityRepository]]


## 문법

- 생성 시 리파지터리 [[클래스(class)]]를 [[extends]] 해준다.

```ts
import { EntityRepository, Repository } from 'typeorm';  
import { Board } from './board.entity';  
  
@EntityRepository(Board)  
export class BoardRepository extends Repository<Board> {


}
```
