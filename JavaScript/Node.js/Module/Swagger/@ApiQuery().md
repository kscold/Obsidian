- [[NestJS]]의 [[Swagger]]를 작성할 때, [[쿼리스트링(Querystring)]]은 자동적으로 인식을 하지 못한다.
- 따라서 [[컨트롤러(Controller)]]의 [[핸들러(Handler)]]에 @ApiQuery() [[데코레이터(Decorator)]]를 붙여서 [[쿼리스트링(Querystring)]]에 대한 보충 설명을 할 수 있다.


## 예시

- 아래와 같이 @ApiQuery() [[데코레이터(Decorator)]]에 [[객체(Object)]]를 넣어 [[쿼리스트링(Querystring)]]의 [[속성(Property)]]들을 보충설명할 수 있다.

```ts
@Controller()  
export class ExampleController {  
	@Get(':id/chats')  
	@ApiQuery({  
	    name: 'perPage',  
	    required: true,  
	    description: '한 번에 가져오는 개수',  
	})  
	@ApiQuery({  
	    name: 'page',  
	    required: true,  
	    description: '불러온 페이지',  
	})  
	getChat(@Query() query, @Param() param) {  
	    console.log(query.perPage, query.page);  
	    console.log(param.id, param.url);  
	}
}
```

- 만약 [[쿼리스트링(Querystring)]]이 여러개라면 name [[속성(Property)]]을 지정하여 특정 [[쿼리스트링(Querystring)]]에 대한 설명 기술할 수 있다.