- [[FindOptions]] 인터페이스([[Interface]])의 [[속성(Property)]]으로 FindOptionsWhere [[속성(Property)]]은 필터링할 조건을 설정할 수 있다.


## [[WHERE]] [[속성(Property)]]

- 기본 사용법은 아래와 같다.

```ts
const users = await userRepository.find({
	where: { isActive: true },
});
```

- 다중 조건 사용법은 아래와 같다.
- 각 [[객체(Object)]] 내부는 and 관계이고, [[객체(Object)]] 외부는 or 관계이다.

```ts
const users = await userRepository.find({
	where: [
		{ firstName: "John", lastName: "Doe" }, // 같은 객체 안은 and 관계
		{ firstName: "Jane", lastName: "Smith" }, 
	]
});
```

- 중첩 조건 사용법은 아래와 같다.
- [[연관 관계(Relationships)]]가 있는 [[속성(Property)]]도 조건을 걸 수 있다.

```ts
const users = await userRepository.find({
	where: {
		isActive: true,
		profile: { age: MoreThan(25) },
	}
});
```
