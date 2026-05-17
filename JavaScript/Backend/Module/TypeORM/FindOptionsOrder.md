- [[FindOptions]] 인터페이스([[Interface]])의 [[속성(Property)]]으로 FindOptionsOrder [[속성(Property)]]을 통해 정렬 조건을 설정할 수 있다.


## 예시

- 단일 필드([[열(Column)]]) 정렬 사용법이다.

```ts
const users = await userRepository.find({
	order: { firstName: "ASC" }
})
```

- 복수 필드([[열(Column)]]) 정렬 사용법이다.

```ts
const users = await userRepository.find({
	order: { lastName: "ASC" ,firstName: "DESC" }
})
```