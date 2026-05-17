- @IsNotEmpty()는 [[class-validator]] [[모듈(Module)]]에서 제공하는 유효성 검사로 해당 필드([[열(Column)]])가 비어있지 않아야 유효한 값으로 처리되는 [[데코레이터(Decorator)]]이다. 

- 반대로 [[@IsEmpty()]]를 가지고 있다.

- [[@IsDefined()]]이 [[null]]과 [[undefined]]만 체크한다면 @IsNotEmpty()는 [[null]]과 [[undefined]]와 `''`(빈 문자열)을 체크한다.



## 예시

-  주로 사용자가 값을 입력해야 하는 필드(예: 이름, 이메일, 비밀번호)에서 사용된다.

```ts
import { IsNotEmpty } from 'class-validator';  

class CreateUserDto {   
	@IsNotEmpty() // 필수 입력 필드   
	username: string;     
	
	@IsNotEmpty() // 필수 입력 필드   
	password: string; 
}
```

- 위 코드에서 `username`과 `password`는 비어있을 수 없으며, 값이 없거나 빈 문자열일 경우 유효성 검증에 실패한다.

```json
{   
	"username": "username should not be empty",   
	"password": "password should not be empty" 
}
```