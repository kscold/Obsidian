- [[NestJS]]에서 사용할 수 있게 만들어 놓은 기본 [[Built-in pipe]] 6가지의 중 하나의 [[파이프(Pipe)]]로, Int 형태인지를 검사한다.
- 보통 [[매개변수(parameter)]] 레벨의 검증에서 쓰인다.


## 문법

- 일반 적인 [[매개변수(parameter)]] 검증 부분의 경우, 아래와 같이, [[@Param()]]이나 [[@Body()]]와 같은 [[데코레이터(Decorator)]]의 두번째 [[매개변수(parameter)]]에 인자로 ParseIntPipe를 넣어주면 된다.

```ts
@Get(':id')  
get(@Param('id', ParseIntPipe){
	// 로직
}
```

- 그러나 검증 실패 [[에러(error)]]를 커스텀하기 위해서는 아래와 같이, [[new]] [[키워드(Keyword)]]로 생성하여, exceptionFactory() [[메서드(Method)]]를 통해, [[에러(error)]] 메세지를 커스텀한다.

```ts
@Get(':id')  
get(
	@Param(
		'id',
		new ParseIntPipe({
			exceptionFactory(error) {
				throw new BadRequestException('숫자를 입력해주세요');
            },
        }),
    )
) {}
```