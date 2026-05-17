- @IsIn() [[데코레이터(Decorator)]]는 [[class-validator]] [[모듈(Module)]]에서 제공하는 유효성 검사 [[데코레이터(Decorator)]]로, 값이 특정 [[배열(Array)]] 내에 포함되어 있는지를 검증할 때 사용된다. 

- 주로 값이 제한된 범위의 값들 중 하나인지 확인해야 할 때 유용하다.

- 필드([[열(Column)]])의 값이 지정된 [[배열(Array)]]에 포함되어 있어야 한다.
- [[배열(Array)]]에 포함되지 않은 값이 들어오면 유효하지 않은 것으로 처리된다.


## 예시

- 주로 선택 가능한 값이 정해져 있을 때 사용한다. (예: 상태 값, 카테고리, 권한 등)

```ts
import { IsIn } from 'class-validator';  

class UpdateUserDto {   
	@IsIn(['admin', 'user', 'guest']) // 이 필드는 'admin', 'user', 'guest' 중 하나여야만 유효   
	role: string; 
}
```

- 위 예시에서는 `role` 필드가 `'admin'`, `'user'`, `'guest'` 중 하나여야만 유효한 값으로 처리된다.
- 다른 값이 들어올 경우 유효성 검증에서 실패한다.

```json
{   
	"role": "role must be one of the following values: admin, user, guest" 
}
```