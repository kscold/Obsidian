- [[커스텀 파이프(Custom Pipe)]]를 만들 때, [[PipeTransform]] 인터페이스([[Interface]]) 안에 transform() [[메서드(Method)]]의 두번째 [[매개변수(parameter)]]의 [[타입 표기(Type Annotation)]]이다.


## ArgumentMetadata의 구성

```ts
export interface ArgumentMetadata {  
    // Indicates whether argument is a body, query, param, or custom parameter
    readonly type: Paramtype;  
	
	// Underlying base type (e. g., String) of the parameter, based on the type definition in the route handler.
	readonly metatype?: Type<any> | undefined;  
	
	// String passed as an argument to the decorator. Example: @Body('userId') would yield userId
	readonly data?: string | undefined;  
}
```

- type의 경우 body 인지, [[쿼리스트링(Querystring)]]인지, [[URL 파라미터(URL Parameter)]]인지 혹은 커스텀 파라미터인지를 검증한다.
- metatype은 [[파이프(Pipe)]]를 적용한 [[매개변수(parameter)]]가 어떤 타입으로 선언되었는지 받아올 수 있다.
- data의 경우 body의 값을 받아올 수 있다.