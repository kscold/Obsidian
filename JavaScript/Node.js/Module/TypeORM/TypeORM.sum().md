- [[TypeORM]]의 sum() [[메서드(Method)]]는 해당되는 필드([[열(Column)]])의 값을 모두 더한다.


## 예시

- 아래 코드와 같이 사용하여, 첫번째 [[매개변수(parameter)]]는 더할 필드([[열(Column)]])명인 age를 명시하고 firstName 필드([[열(Column)]])이 "Timber"인 필드([[열(Column)]])들을 전부 합친다.

```ts
const sum = await repository.count(
	"age",
	{ firstName: "Timber" }
); 
```

​