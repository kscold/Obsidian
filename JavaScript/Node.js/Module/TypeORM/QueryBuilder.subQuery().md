- [[TypeORM]]에서 [[QueryBuilder]]를 [[쿼리(Query)]]를 실행할 때, 내부적으로 한번 더 [[쿼리(Query)]]를 실행하는 방법이다.
- [[QueryBuilder.where()]]에서 [[화살표 함수(Arrow function)]]를 사용하여 순서대로 [[쿼리(Query)]]를 실행한다.


## 예시

- 아래와 같이 서브 쿼리를 실행할 때에는 subQuery()로 [[쿼리(Query)]]를 시작하며, 반환할때에는 getQuery() [[메서드(Method)]]를 통해 [[쿼리(Query)]]의 실행결과를 반환한다.

```ts
const users = await connection.getRepository(User)
	.createQueryBuilder("user")
	.where(qb => {
		const subQuery = qb.subQuery()
			.select("subUser.id")
			.from(User, "subUser")
			.where("subUser.isActive = :isActive", { isActive: true })
			.getQuery();
		return "user.id IN " + subQuery; 
	})
	.setParameter("isActive", true)
	.getMany();
```
