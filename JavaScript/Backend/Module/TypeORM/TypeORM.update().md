- [[TypeORM]]의 update() [[메서드(Method)]]를 사용하면 바꾸고싶은 특정 필드([[열(Column)]])를 수정([[UPDATE]])할 수 있다.


## 문법

- 첫번째 [[매개변수(parameter)]]에는 검색 조건을 입력해주고 두번째 [[매개변수(parameter)]]는 변경 필드([[열(Column)]])를 입력해 준다.


## 예시


```ts
await respository.update(
	{ age: 18 },
	{ category: "ADULT" },
)
```


```sql
UPDATE user
SET category = ADULT
WHERE age = 18
```

- 아래 처럼 필드([[열(Column)]])을 명시하지 않으면 기본적으로 [[기본 키(Primary Key)]]를 가리킨다.

```ts
await respository.update(
	1,
	{ firstName: "Code Factory" },
)
```


```sql
UPDATE user
SET firstName = Code Factory
WHERE id = 1
```