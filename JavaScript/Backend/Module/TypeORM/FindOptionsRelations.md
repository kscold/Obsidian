- [[FindOptions]] 인터페이스([[Interface]])의 [[속성(Property)]]으로 FindOptionsRelations [[속성(Property)]]을 통해 [[테이블(Table)]] 간의 통해 [[연관 관계(Relationships)]] 설정할 수 있다.


## 예시

```ts
const users = await userRepository.fine({
	relations: ["profile", "photos"],
})
```