- 커스텀 파이프는 다양한 용도로 사용될 수 있다.

- 예를들어, 입력 데이터의 유효성검사, 데이터 변환, 비즈니스 로직 처리 등이 가능하다. 
- 이에 따라 [[파이프(Pipe)]] 로직도 다양하게 작성될수도 있다.

- 커스텀 파이프를 만드려면 [[PipeTransform]] 인터페이스([[interface]])를 구현하는 [[클래스(class)]]를 작성해야 한다. 



- 이 [[메서드(Method)]]는 [[NestJS]]가 인자를 처리하고 [[파이프(Pipe)]]가 변환하거나 검증할 값을 입력으로 받고 변환된 값을 반환하기 위해 사용된다.


## 커스텀 파이프 선언 방법

- [[NestJS]]의 [[의존성 주입(Dependency Injection)]]를 사용하기 위해 [[@Injectable()]]를 한다.
- [[PipeTransform]] 인터페이스([[interface]])는 [[transform()]] [[메서드(Method)]]를 정의한다.
- [[ArgumentMetadata]] [[메서드(Method)]]의 [[매개변수(parameter)]] 정보를 포함하며, [[메서드(Method)]]로 전달되는 인자의 메타데이터를 나타낸다.

- BadRequestException 등을 통해 유효성 검사 실패시 던질 예외 [[HTTP 응답 상태(status)]] 400 등을 반환한다.

```ts
import { Injectable, PipeTransform, ArgumentMetadata, BadRequestException} from '@nestjs/common';


@Injectable() // 의존성 주입 가능 클래스 지정
export class ParseIntPipe implements PipeTransform<string, number>{
	// transform 메서드 구현
	transform(value: string, metadata: ArgumentMetadata): number{
		const val = parseInt(value, 10); // 입력 값을 정수로 변환, 10진수 사용
		
		if(isNaN(val)){ // '숫자'가 아닌 경우
			throw new BadRequestException('검증에 실패했습니다'); // 예외처리
		}
		
		return val; // 변환된 정수 반환
	}
}
```