- [[NestJS]]의 [[Swagger]]를 작성할 때, [[URL 파라미터(URL Parameter)]]은 자동적으로 인식을 하지 못한다.
- 따라서 [[컨트롤러(Controller)]]의 [[핸들러(Handler)]]에@ApiParam() [[데코레이터(Decorator)]]를 붙여서 [[URL 파라미터(URL Parameter)]]에 대한 보충 설명을 할 수 있다.

- [[@ApiQuery()]]와 사용법이 비슷하다.


## 예시

- 아래와 같이 @ApiParam()) [[데코레이터(Decorator)]]에 [[객체(Object)]]를 넣어 [[URL 파라미터(URL Parameter)]]의 [[속성(Property)]]들을 보충설명할 수 있다.

```ts
@Controller(':url')  
export class ExampleController {
	
	@Get(':id')  
	@ApiParam({  
	    name: 'url',  
	    required: true,  
	    description: '워크스페이스 url',  
	})  
	@ApiParam({  
	    name: 'id',  
	    required: true,  
	    description: '사용자 아이디',  
	})
    getSome(@Param() param) { 
	    console.log(param.id, param.url);
    }
	
}
```