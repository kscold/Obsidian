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


## 연산자 메서드

- [[WHERE]] 조건을 걸때 사용할 수 있는 연산자 [[메서드(Method)]]들은 아래와 같다.
### Equal

- 같은 값을 찾을 때 사용한다.

```ts
const users = await userRepository.find({
	where: { age: Equal(25) },
})
```
### Not

- 아닌 값을 찾을 때 사용한다.

```ts
const users = await userRepository.find({
	where: { age: Not(25) },
})
```
### LessThan & LessThanOrEqual

- 적은 값이나 적거나 같은 값을 찾을 때 사용한다.

```ts
const users = await userRepository.find({
	where: { age: LessThan(30) },
})

const users = await userRepository.find({
	where: { age: LessThanOrEqual(30) },
})
```
### MoreThan & MoreThanOrEqual

- 큰 값이나 크거나 같은 값을 찾을 때 사용한다.

```ts
const users = await userRepository.find({
	where: { age: MoreThan(20) },
})

const users = await userRepository.find({
	where: { age: MoreThanOrEqual(20) },
})
```
### Between

- 사이 값을 찾을 때 사용한다.

```ts
const users = await userRepository.find({
	where: { age: Between(20, 30) },
})
```
### Like & ILike

- 스트링에 매칭되는 값을 찾을 때 사용한다.
- Like는 대소문자를 구분할 때, ILike는 대소문자 구분하지 않을 때 사용한다.

```ts
const users = await userRepository.find({
	where: { firstName: Like("Co%") }, // %는 어떤 값이든 들어올 수 있는 와일드 카드
})

const users = await userRepository.find({
	where: { firstName: ILike("co%") },
})
```
### In

- 리스트에 매칭되는 값을 찾는다.

```ts
const users = await userRepository.find({
	where: { age: In([10, 25, 30]) },
})
```
### ArrayContains & ArrayContainedBy & ArrayOverlap Operator

- ArrayContains는 [[엔티티(Entity)]] 리스트 값이 타겟 리스트와 완전 똑같은 경우를 필터링한다.
- ArrayContainedBy는 [[엔티티(Entity)]] 리스트 값이 타켓 리스트 안에 모두 포함되는 경우를 필터링한다.
- ArrayOverlap는 [[엔티티(Entity)]] 리스트 값이 타겟 리스트와 겹치는 경우를 필터링한다.

- 아래 코드에서 roles는 [[배열(Array)]]로 선언했다고 가정한다.
- 그러나 [[SQL(Structured Query Language)]]레벨에서 [[배열(Array)]]을 사용하는 경우는 거의 없기 때문에 사용은 비추천이다.

```ts
const users = await userRepository.find({
	where: { roles: ArrayContains(["admin"]) }, // admin과 완전 같은 경우
})


const users = await userRepository.find({
	where: { roles: ArrayContainedBy(["admin", "user"]) }, // admin과 user가 모두 포함되는 경우
})

const users = await userRepository.find({
	where: { roles: ArrayOverlap(["admin", "guest"]) }, // admin와 guest 중 겹치는 값이 있는 경우
})
```

- 특정 정보와 코드 예시가 같다고 가정한다.

```json
// Record 1
{
	id: 1,
	tags: ["admin", "user", "manager"]
}

// Record 2
{
	id: 2,
	tags: ["user", "guest"]
}
```

```ts
const users = await getRepository(User).find({
	tags: ArrayContains(["user", "guest"])
}) // Record 2의 Tags가 완전히 같으니 Record 2만 반환
```

```ts
const users = await getRepository(User).find({
	tags: ArrayContainedBy([
		"admin",
		"user",
		"guest",
		"manager",
	])
}) // Record 1과 Record 2의 Tags들이 모두 필터 리스트에 포함되기 때문에 둘 다 반환
```

```ts
const users = await getRepository(User).find({
	tags: ArrayContainedBy(["guest", "admin"])
}) // Record 1의 Tags와 Record 2의 Tags가 모두 필터 리스트와 겹치는 부분이 있기 때문에 둘 다 반환
```
### IsNull

- [[null]]인 값을 필터링할 때 사용한다.

```ts
const users = await userRepository.find({
	where: { profile: IsNull() },
})
```
### Or

- OR 로직 연산을 할 때 사용한다.

```ts
const loadedPosts = await dataSource.getRepository(Post).findBy({
	title: Or(Equal("About #2"), ILike("About%")), // Equal("About #2") 또는 ILike("About%")
})
```
### And

- AND 로직 연산을 할 때 사용한다.

```ts
const loadedPosts = await dataSource.getRepository(Post).findBy({
	title: And(Not(Equal("About #2")), ILike("%About%")), // Not(Equal("About #2"))와 ILike("%About%")
})
```