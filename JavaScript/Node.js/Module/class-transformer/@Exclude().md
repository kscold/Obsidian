- @Exclude() [[데코레이터(Decorator)]]는 [[class-transformer]]에서 제공하는 [[데코레이터(Decorator)]] 중 하나로, [[클래스(class)]]의 특정 [[속성(Property)]]을 [[직렬화(Serialization)]] 또는 [[역직렬화 (Deserialization)]] 시 제외하고 싶을 때 사용된다.


## 문법

```ts
@Column()  
@Exclude({ toPlainOnly: true })
password: string;
```

### toPlainOnly

- 응답 할때만 제외하고 싶을 때 사용하는 옵션이다.
##  toClassOnly

- 요청 할때만 제외하고 싶을 때 사용하는 옵션이다.