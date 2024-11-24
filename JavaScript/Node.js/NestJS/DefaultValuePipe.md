- [[NestJS]]에서 사용할 수 있게 만들어 놓은 기본 [[Built-in pipe]] 6가지의 중 하나의 [[파이프(Pipe)]]로, [[@Param()]]이나 [[@Body()]]와 같은 [[데코레이터(Decorator)]]가 주어지지 않았을 때, 기본으로 들어갈 값을 지정하는 [[파이프(Pipe)]]이다.
- 보통 [[매개변수(parameter)]] 레벨의 검증에서 쓰인다.


## 문법


```ts
@Get(':id')  
get(@Param('id', DefaultValuePipe(10)){ // id가 주어지지 않았을 때, 기본으로 10으로 설정
	// 로직
}
```