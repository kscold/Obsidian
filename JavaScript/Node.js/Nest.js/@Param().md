- [[Node.js/Nest.js/NestJS]]에서 [[providers]]의 @GET 요청일 때, [[URL 파라미터]]에 값을 추출하여 가져올 때 사용하는 [[데코레이터(Decorator)]]이다.


## 문법

- 아래 코드처럼 [[URL 파라미터]]명을 설정하고 이를 [[변수(Variable)]]화 하여 [[메서드(Method)]] 내부에서 사용한다.

```ts
@Get('/:id')  
getBoardById(@Param('id') id: string): Board {  
    return this.boardsService.getBoardById(id);  
}
```