- [[TypeORM]]을 사용해서 [[데이터베이스(DataBase)]]에서 조건에 맞는 데이터([[행(Row)]])가 존재하는지 boolean 값을 반환 받을 수 있다.


## 예시

```ts
const exists = await repository.exists({
	where: {
		firstName: "Timber",
	}
})
```