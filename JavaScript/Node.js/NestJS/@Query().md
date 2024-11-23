- [[NestJS]] [[데코레이터(Decorator)]] [[쿼리스트링(Querystring)]]에서 필드 데이터를 추출한다.
- 추출된 데이터는 [[컨트롤러(Controller)]] [[메서드(Method)]]드의 [[매개변수(parameter)]]로 전달된다.

- 따라서 주로 GET 요청에서 사용된다.


## 문법

- 아래 코드처럼 @Query() [[데코레이터(Decorator)]]에 아무것도 명시하지 않으면 모든 [[쿼리스트링(Querystring)]]  데이터를 [[매개변수(parameter)]]로 가져올 수 있다.

```ts
@Get('/example')  
getExample(@Query() query) {  
    console.log(query.dataOne, query.dataTwo);
}
```

- 아래 코드처럼 @Query() [[데코레이터(Decorator)]]에  [[속성(Property)]]명을 명시하면 지정된 [[쿼리스트링(Querystring)]] 데이터를 [[매개변수(parameter)]]로 가져올 수 있다.

```ts
@Get('/example')  
getExample(@Query("dataOne") dataOne, @Query("dataTwo") dataTwo) {  
    console.log(dataOne, dataTwo);
}
```