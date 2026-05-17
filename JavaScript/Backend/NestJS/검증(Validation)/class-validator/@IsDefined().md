- @IsDefined()는 [[class-validator]] [[모듈(Module)]] 제공하는 [[데코레이터(Decorator)]]로, 해당 값이 정의되어 있어야 함을 검증하는 데 사용된다.

- 즉, 값이 [[null]]이거나 [[undefined]]가 아닌지를 확인하는 역할을 한다.
- 빈 문자열까지 처리할려면 [[@IsNotEmpty()]]와 [[@IsEmpty()]]를 사용한다.


## 예시

- @IsDefined()는 값이 존재하지 않으면(정의되지 않았으면) 검증을 실패하게 만든다.
- 주로 필수 입력 값에 대해 사용되며, [[null]]이나 [[undefined]] 상태인 경우에 유효하지 않다는 오류 메시지를 반환한다.

```ts
import { IsDefined } from 'class-validator';

class CreateUserDto {
	@IsDefined() // 반드시 값이 정의되어 있어야 함
	username: string;
	
	@IsDefined() // 반드시 값이 정의되어 있어야 함
	password: string;
}
```