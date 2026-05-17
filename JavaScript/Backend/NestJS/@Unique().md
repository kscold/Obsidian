- [[NestJS]]에서 [[엔티티(Entity)]]나 [[DTO(Data Transfer Object)]]의 특정 필드([[열(Column)]])을 유니크하게 만들기 위해서 사용하는 [[데코레이터(Decorator)]]이다.


## 문법

```ts
@Unique(['필드명'])
```

- 위와 같은 형식으로  [[엔티티(Entity)]]나 [[DTO(Data Transfer Object)]] 필드([[열(Column)]])의 제한을 줄 수 있다.


## @Unique 에러 방법

- 일단 오류 코드 확인을 위해 아래와 같이 [[try catch]] 구문을 통해 error 콘솔을 찍어본다.

```ts
try {  
    await this.save(user);  
} catch (error) {  
    console.log('error', error);  
}
```

- 이후 로그에 찍힌 콘솔을 기준으로