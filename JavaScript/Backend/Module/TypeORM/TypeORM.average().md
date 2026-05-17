- [[TypeORM]]의 average() [[메서드(Method)]]는 해당되는 필드([[열(Column)]])의 값의 평균을 구한다.


## 예시

- 아래 코드와 같이 사용하여, 첫번째 [[매개변수(parameter)]]는 평균을 저장할 필드([[열(Column)]])명인 age를 명시하고 firstName 필드([[열(Column)]])이 "Timber"인 필드([[열(Column)]])들의 평균을 구한다.

```ts
const sum = await repository.average(
	"age",
	{ firstName: "Timber" }
); 
```

​