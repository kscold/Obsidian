- [[FindOptions]] 인터페이스([[Interface]])의 [[속성(Property)]]으로 FindOptionsSelect [[속성(Property)]]을 통해 [[테이블(Table)]] 특정 필드([[열(Column)]])의 데이터([[행(Row)]])을 조회할 수 있다.


## 예시

```ts
const users = await userRepository.find({
	select: ["firstName", "lastName"],
})
```