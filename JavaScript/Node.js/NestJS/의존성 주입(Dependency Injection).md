- 의존성 주입은 제어의 역전(Incersion of Control) 기술 중 하나이다.

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