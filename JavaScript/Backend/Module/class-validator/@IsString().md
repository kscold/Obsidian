- [[class-validator]]에서 문자열인지 검증하는 [[데코레이터(Decorator)]]이다.


## @IsString() [[데코레이터(Decorator)]]와 : string [[타입 표기(Type Annotation)]]의 차이

- @IsString() [[데코레이터(Decorator)]]는 [[런타임(runtime)]]에서 값의 유효성을 검사하고, : string [[타입 표기(Type Annotation)]]는  [[타입스크립트(TypeScript)]] 컴파일러가 컴파일 시간에 타입을 확인하도록 도와준다.
- 두 가지는 서로 다른 목적을 가지고 있지만, 함께 사용하면 런타임과 컴파일 시간에 각각 유효성 검사와 타입 검사를 수행하여 안전한 코드를 작성할 수 있다.

### @IsString()

- @IsString()은 [[class-validator]] [[모듈(Module)]]에서 제공하는 [[데코레이터(Decorator)]]로, 해당 필드의 값이 문자열(string) 형식인지를 검증한다.
- 즉, 입력된 값이 문자열이 아닌 경우 유효성 검사를 통과하지 못하게 된다.
- 이 데코레이터를 사용하여 [[DTO(Data Transfer Object)]] [[클래스(class)]]의 name 필드가 문자열 형식으로 제공되어야 함을 나타낸다.

### : string
- : string은 [[타입스크립트(TypeScript)]]에서 [[변수(Variable)]] 또는 [[함수(Function)]] [[매개변수(parameter)]]의 타입을 명시하는 방법 중 하나이다

- 이 경우, name 필드가 문자열(string) 타입의 값을 가지는 것을 [[타입스크립트(TypeScript)]]에 알려준다.
- [[타입스크립트(TypeScript)]] 컴파일러는 이 정보를 사용하여 코드의 정적 타입 검사(static type checking)를 수행하고, 올바른 타입이 아닌 경우에는 오류를 발생시킨다.

