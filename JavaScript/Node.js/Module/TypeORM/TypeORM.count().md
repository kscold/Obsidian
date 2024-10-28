- [[TypeORM]]의 count() [[메서드(Method)]]는 해당되는 갯수를  반환한다.


## 예시

- 아래 코드와 같이 [[FindOptionsWhere]] 속성을 사용하여, firstName 필드([[열(Column)]])이 "Code Factory"인 문자열의 갯수를 반환한다.

```ts
const count = await repository.count({
		where: {
			firstName: "Code Factory",
		}
	}
}); 
```

​