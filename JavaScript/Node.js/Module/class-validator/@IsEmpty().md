- @IsEmpty()는 [[class-validator]] [[모듈(Module)]]에서 제공하는 유효성 검사로 해당 필드([[열(Column)]])가 반드시 비어있어야 유효한 값으로 처리되는 [[데코레이터(Decorator)]]이다. 

- 반대로 [[@IsNotEmpty()]]를 가지고 있다.

- [[@IsDefined()]]이 [[null]]과 [[undefined]]만 체크한다면 @IsEmpty()는 [[null]]과 [[undefined]]와 `''`(빈 문자열)을 체크한다.
- `''`(빈 문자열)의 경우는 비어있는 것이므로 당연히 @IsEmpty()의 경우 통과가 된다.


## 예시

- 보통 사용자가 값을 입력하지 말아야 하는 경우(예: 자동으로 채워지는 필드 또는 생성 시에 사용되지 않는 필드)에서 사용된다.

```ts
import { IsNotEmpty } from 'class-validator';  

class AdminUpdateDto {
	@IsEmpty() // 필드는 비어 있어야 함 (예: 자동 생성 필드)
	adminRole: string;
}

```

- 위 코드에서 `adminRole` 필드는 반드시 비어 있어야만 유효성 검증을 통과한다.

```json   
{
	"adminRole": "adminRole must be empty"
}
```