- validation pipe은 [[class-validator]] 패키지와 해당 패키지에서 제공하는 데코레이터를 활용하여 요청 데이터의 유효성을 검증하는 기능이다.

- 이를 통해 개발자는 간단한 [[데코레이터(Decorator)]]를 사용하여 요청 데이터에 대한 검증 규칙을 선언하고, 이를 통해 모든 클라이언트 요청 데이터에 대한 유효성 검사를 간편하게 수행할 수 있다.


## 예시

- 아래 코드는 [[DTO(Data Transfer Object)]]에 ValidationPipe를 적용하는 예시이다.

```ts
interface CreateUserDto {
    firstName: string;
    lastName: string;
    email: string;
    password: string;
}
```

- 이를 [[class-validator]]를 이용해 검증하려면 다음과 같이 코드를 작성한다.

```ts
import { IsString, IsEmail } from 'class-validator';

export class CreateUserDto {
    @IsString()
    readonly firstName: string;
	
    @IsString()
    readonly lastName: string;
	
    @IsEmail()
    readonly email: string;
	
    @IsString()
    readonly password: string;
}
```

- 여기서 [[@IsString]]이나 [[@IsEmail()]]과 같은 데코레이터는 해당 클래스 프로퍼티가 맞는 형식인지 검증한다.  
- 이를 사용하기 위해서는 [[class-validator]]에서 제공하는 [[데코레이터(Decorator)]]를 불러와 사용하면 된다.