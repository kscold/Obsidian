- class-transformer는 [[타입스크립트(TypeScript)]]와 JavaScript에서 [[객체(Object)]]를 [[직렬화(Serialization)]]하고 [[역직렬화 (Deserialization)]]하는 데 사용되는 라이브러리이다. 

- 이 [[모듈(Module)]]은 [[객체(Object)]]를 다른 형식으로 변환하거나 평범한 자바스크립트 [[객체(Object)]]를 [[클래스(class)]]로 변환하는 작업을 쉽게 처리할 수 있도록 도와준다.

- 주로 [API 요청과 응답 데이터를 처리할 때 유용하며, [[클래스(class)]]에 정의된 구조를 유지하면서 데이터를 변환할 수 있게 해준다.


## class-transformer [[데코레이터(Decorator)]]

- class-transformer는 여러 [[데코레이터(Decorator)]]를 제공하여 클래스 변환 시 어떤 방식으로 변환할지 정의할 수 있다.

### [[@Transform()]]

- 특정 [[속성(Property)]]을 변환하는 방식을 커스텀할 때 사용된다.
### [[@Expose()]]

- [[직렬화(Serialization)]] 또는 [[역직렬화 (Deserialization)]] 시 노출될 필드([[열(Column)]])를 정의한다.
### [[@Exclude()]] 

- [[직렬화(Serialization)]] 또는 [[역직렬화 (Deserialization)]] 시 무시될 필드([[열(Column)]])를 정의한다.
### [[@Type()]]

- 특정 [[속성(Property)]]의 타입을 명시적으로 지정하여 [[직렬화(Serialization)]] 및 [[역직렬화 (Deserialization)]]할 때 해당 타입으로 변환되도록 설정한다.


## 문법

- class-transformer를 사용하기 위해서는 적용하고자 하는 [[컨트롤러(Controller)]]에 [[@UseInterceptors()]] [[데코레이터(Decorator)]]를 적용하면 된다.

```ts
@Controller('somting')  
@UseInterceptors(ClassSerializerInterceptor) // class-transformer를 사용하기 위해 적용
export class somtingController {
	...
}
```