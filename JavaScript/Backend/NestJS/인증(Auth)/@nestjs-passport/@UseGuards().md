- @UseGuards()는 @nestjs/passport([[Passport Module]])에서 가져온 [[AuthGuard()]]를 이용하면 요청(Request) 안에 유저 정보를 넣어 줄 수 있다.


## 문법

- 아래 코드와 같이 사용하고 싶은 [[메서드(Method)]]위에 @UseGuards() [[데코레이터(Decorator)]]를 붙이고, [[AuthGuard()]]를 사용한다.

```ts
@Post()
@UseGuards(AuthGuard())
Method() {
	
}
```


## 예시

- 실제로 정의한 [[AuthGuard()]]를 주입하면, 실제 유저에 대한 정보가 [[@Req]] [[데코레이터(Decorator)]]를 사용한 req에 유저 정보가 들어가게 된다.

```ts
@Post('/authTest')
@UseGuards(AuthGuard())
authTest(@Req() req) {
	console.log(req);
}
```