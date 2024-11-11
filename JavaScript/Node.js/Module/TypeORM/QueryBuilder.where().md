- [[TypeORM]]에서 [[QueryBuilder]]를 실행할 때 특정 조건 즉, [[WHERE]]를 사용할 수 있는 [[메서드(Method)]]이다.


## 예시

- where()와 andWhere()를 사용하여 하나 혹은 이상의 필터링을 조건을 적용할 수 있다.

```ts
const users = await connection.getRepository(User)
	.createQueryBuilder("user")
	.where("user.isActive = :isActive", { istActive: true })
	.getMany();
```

- 아래는 여러 조건을 사용할 때 예시이다.

```ts
const users = await connection.getRepository(User)
	.createQueryBuilder("user")
	.where("user.firstName = :firstName", { firstName: "John" })
	.andWhere("user.lastName = :lastName", { firstName: "Doe" })
	.getMany();
```