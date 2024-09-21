- [[NestJS]]의 [[Swagger]]를 작성할 때, [[컨트롤러(Controller)]]의 [[핸들러(Handler)]]에 @ApiOperation() [[데코레이터(Decorator)]]를 붙여 [[핸들러(Handler)]]에 대한 보충설명을 할 수 있다.


## 예시

- 아래와 같이 @ApiOperation() [[데코레이터(Decorator)]]에 [[객체(Object)]]를 넣어 [[속성(Property)]]들을 보충설명할 수 있다.

```js
export class UsersController {  
    constructor(private readonly usersService: UsersService) {}  
  
    @Get()  
    @ApiOperation({ summary: '내 정보 조회' })  
    getUsers(@Req() req) {  
        return req.user;  
    }
```