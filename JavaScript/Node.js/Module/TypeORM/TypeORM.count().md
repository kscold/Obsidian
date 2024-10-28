- [[TypeORM]]의 count() [[메서드(Method)]]는 해당되는 갯수를  반환한다.


## 예시

```ts
const count = await repository.count({
		where: {
			firstName: "Code Factory",
		}
	}
}); 
```

​