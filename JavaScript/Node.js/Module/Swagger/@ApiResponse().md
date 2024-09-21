- [[NestJS]]의 [[Swagger]]를 작성할 때, [[컨트롤러(Controller)]]의 [[핸들러(Handler)]]에 대한 응답에 대한 성공, 실패, 개체정의를 @ApiResponse() [[데코레이터(Decorator)]]를 통해 할 수 있다.


## 예시

- 아래처럼 정의하여 성공(200)했을 때와 실패(500)일 때의 [[HTTP 응답 상태(status)]]와 응답 [[DTO(Data Transfer Object)]] [[객체(Object)]]를 정의할 수 있다.

```ts
@Controller('api/users')  
@ApiTags('USER')  
export class UsersController {

	@ApiOperation({ summary: '로그인' })  
	@ApiResponse({ status: 200, description: '성공', type: UserDto })  
	@ApiResponse({ status: 500, description: '서버 에러' })  
	@Post('login')  
	login(@Req() req) {  
	    return req.user;  
	}

}
```