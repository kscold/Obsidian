- ValidationPipe은 [[class-validator]] 패키지와 해당 패키지에서 제공하는 [[데코레이터(Decorator)]]를 활용하여 요청 데이터의 유효성을 검증하는 기능이다.

- 이를 통해 개발자는 간단한 [[데코레이터(Decorator)]]를 사용하여 요청 데이터에 대한 검증 규칙을 선언하고, 이를 통해 모든 클라이언트 요청 데이터에 대한 유효성 검사를 간편하게 수행할 수 있다.
- 즉, 요청이 [[컨트롤러(Controller)]]에 있는 [[핸들러(Handler)]]로 들어왔을 때, [[DTO(Data Transfer Object)]]에 있는 유효성 조건에 맞게 체크를 해주려면 ValidationPipe를 사용하면 된다.


## 문법

- `main.ts`에 아래와 같이 ValidationPipe [[객체(Object)]]를 전역으로 적용시킴으로써 [[class-validator]]가 사용 가능하다.
```ts
import { NestFactory } from '@nestjs/core';  
import { AppModule } from './app.module';  
import { ValidationPipe } from '@nestjs/common';  
  
async function bootstrap() {  
    const app = await NestFactory.create(AppModule);  
	  
    app.useGlobalPipes(new ValidationPipe({ 
	    whitelist: true, forbidNonWhitelisted: true 
	})); 
	  
    await app.listen(8080);  
}  
  
bootstrap();
```

- 이때, whitelist: true 옵션을 줌으로써 [[DTO(Data Transfer Object)]]나 [[엔티티(Entity)]]에 정의되지 않은 [[속성(Property)]]을 무시할 수 있다.
- forbidNonWhitelisted: true 옵션은 있으면 안되는 [[속성(Property)]]이 존재했을 때, [[에러(error)]]를 반환한다.


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

- 여기서 [[@IsString()]]이나 [[@IsEmail()]]과 같은 [[데코레이터(Decorator)]]는 해당 클래스 프로퍼티가 맞는 형식인지 검증한다.  
- 이를 사용하기 위해서는 [[class-validator]]에서 제공하는 [[데코레이터(Decorator)]]를 불러와 사용하면 된다.