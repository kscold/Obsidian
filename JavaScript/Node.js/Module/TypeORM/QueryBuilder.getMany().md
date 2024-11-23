- [[TypeORM]]에서 [[쿼리빌더(QueryBuilder)]]를 실행한 결과가 여러개일 때, 사용하는 [[메서드(Method)]]이다.
- 즉, 복수 데이터([[행(Row)]])만 가져올 때 사용한다.


## 예시

```ts
const users = await connection.getRepository(User)
	.createQueryBuilder("user")
	.select(["user.id", "user.firstName", "user.lastName"])
	.getMany();
```
