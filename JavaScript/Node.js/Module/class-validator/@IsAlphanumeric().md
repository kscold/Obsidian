- [[class-validator]]에서 해당 필드([[열(Column)]])의 해당 값이 영문자(A-Z, a-z)와 숫자(0-9)로만 구성된 문자열인지 확인 하는 [[데코레이터(Decorator)]]이다.

- 즉, 특수문자나 공백이 없어야 유효한 값으로 간주된다.


## 예시    

- @IsAlphanumeric() [[데코레이터(Decorator)]]는 아래 코드처럼 주로 사용자 이름이나 코드 등의 필드에서 문자와 숫자만 허용할 때 사용된다.

```ts
@IsAlphanumeric()
username: string;
```

- username 필드는 영문자와 숫자로만 구성된 문자열이어야 한다.
- 예를 들어, `User123`은 유효하지만, `User_123`이나 `User 123`은 유효하지 않는다.