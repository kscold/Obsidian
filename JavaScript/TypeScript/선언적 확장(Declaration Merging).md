- 선언적 확장(Declaration Merging)은  [[타입스크립트(TypeScript)]]에서 같은 이름의 [[interface]]를 선언하면, 자동으로 확장되는 것을 의미한다.


## 예시

```ts
interface Person {
	name: string;
	age: number;
}

interface Person { // 선언적 확장
	gender: string;
}

const jieun: Person = {
	name: 'jieun',
	age: 27,
	gender: 'female'
}
```