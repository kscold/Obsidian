- @Equals() [[데코레이터(Decorator)]]는 [[class-validator]] [[모델(model)]]에서 제공하는 유효성 검사 [[데코레이터(Decorator)]]로, 특정 값과 같아야 하는지 검증할 때 사용된다. 
- 반대로 [[@NotEquals()]] [[데코레이터(Decorator)]]도 있다.

- 즉, 필드([[열(Column)]]) 값이 지정된 값과 정확히 일치하는지 확인한다.

- @Equals()는 해당 필드([[열(Column)]])가 주어진 값과 같을 때만 유효한 것으로 간주한다.
- 비교할 값은 [[데코레이터(Decorator)]]의 인자로 전달된다.
- 주로 비밀번호 확인이나 특정 상수 값과 비교할 때 유용하게 사용된다.

## 예시


```ts
import { Equals } from 'class-validator';  

class ConfirmUserDto {   
	@Equals('CONFIRMED') // 값이 'CONFIRMED'와 같아야 함   
	status: string; 
}
```

- 위의 코드에서 status 값은  'CONFIRMED' 와 같아야만 유효하게 처리된다.
- 그렇지 않으면 검증에 실패한다.