- [[TypeORM]]에서 preload()는 [[데이터베이스(DataBase)]]에 저장된 값을 [[기본 키(Primary Key)]] 기준으로 불러오고 입력된 [[객체(Object)]]의 값으로 [[속성(Property)]]을 덮어 쓴다.

- 덮어쓰는 과정에서 [[데이터베이스(DataBase)]]에 [[UPDATE]] 요청이 보내지지 않는다.


## 예시

```ts
const partialUser = {
	id: 1,
	firstName: "Code Factory",
	profile : {
		id: 1,
	}
}

const user = await repository.preload(partialUser)
```