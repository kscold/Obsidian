- [[TypeORM]]을 사용해서 [[데이터베이스(DataBase)]]에서 조건에 맞는 데이터([[행(Row)]])와 전체 갯수를 반환한다.


## 예시

```ts
const [rows, count] = await repository.findAndCount({
	where: {
		firstName: "Code Factory",
	}
})
```