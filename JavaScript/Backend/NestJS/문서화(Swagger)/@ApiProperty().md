- [[NestJS]]의 [[Swagger]]를 작성할 때, [[DTO(Data Transfer Object)]]의 [[속성(Property)]]을 보충설명할 때 사용하는 [[데코레이터(Decorator)]]이다.


## 예시

- 아래와 같이 @ApiProperty() [[데코레이터(Decorator)]]에 [[객체(Object)]]를 넣어 [[속성(Property)]]들을 보충설명할 수 있다.

```js
export class ExampleDto {
	@ApiProperty({  
	    example: 'example@gmail.com', // 예시 속성  
	    description: '이메일', // 속성 설명
	    required: true, // 필수 옵션 유무
	})
	public email: string;
}
```