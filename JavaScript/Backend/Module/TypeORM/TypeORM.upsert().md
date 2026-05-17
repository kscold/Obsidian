- [[TypeORM]]의 upsert() [[메서드(Method)]]는 [[TypeORM.update()]]와 [[TypeORM.insert()]]를 합친 [[메서드(Method)]]이다.

- 데이터([[행(Row)]]) 생성 시도를 한 후 만약에 이미 존재하는 데이터([[행(Row)]])이라면 업데이트를 진행한다.

- [[TypeORM.save()]]와 다르게 upsert()는 하나의 [[트랜잭션(Transaction)]]에서 작업이 실행된다.


## 문법

```ts
await repository.upsert(
	entity,
	['column']
)
```

- 첫번째 [[매개변수(parameter)]]로 [[엔티티(Entity)]]를 넘겨주고, 두번째 [[매개변수(parameter)]]로는 [[열(Column)]]을 넘겨주는 방식으로 사용한다.


## 예시

```ts
await repository.upsert(
	[
		{ externalId: "abc123", firstName: "Code" },
		{ externalId: "bca321", firstName: "Factory" },
	],
	["externalId"]
)
```

- 아래는 변환되어서 실행되는 [[SQL(Structured Query Language)]] [[쿼리(Query)]]문이다.

```sql
INSERT INTO user
VALUES
	(externalId = "abc123", firstName = "Code"),
	(externalId = "bca321", firstName = "Factory"),
ON CONFLICT (externalId) DO UPDATE firstName = EXCLUDED.firstName

```