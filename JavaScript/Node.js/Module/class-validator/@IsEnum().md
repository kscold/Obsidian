- @IsEnum() [[데코레이터(Decorator)]]는 [[enum]](열거형) 타입의 값을 검증하는 데 사용된다.

- 해당 필드([[열(Column)]])의 값이 지정된 [[enum]] 값 중 하나인지 확인할 수 있다. 
- 이 [[데코레이터(Decorator)]]는 주로 [[enum]] 타입을 사용해 미리 정의된 값만 허용하고 싶을 때 유용하다.

- [[@IsInt()]]과 비슷하다.


## 예시

```ts
enum UserRole {   
	Admin = 'admin',   User = 'user',   Guest = 'guest', 
}  

class UserDto {   
	@IsEnum(UserRole)   
	role: UserRole; 
}
```