- [[타입스크립트(TypeScript)]]에서 단순한 [[원시 타입(Primitive type)]]이나 튜플([[tuple]]), 유니언([[Union]]) 타입을 선언할 때 type을 사용하는 것이 좋다.

- [[객체(Object)]] 타입을 정의할 때도 사용할 수 있지만, [[객체(Object)]] 타입을 정의할 때는 [[interface]]를 사용하는게 좋다.
- & 기호를 이용해서 확장할 수 있다.


## 예시

```ts
type Name = string; // primitive
type Age = number;
type Person = [string, number, boolean]; // tuple
type NumberString = string | number; // union
```


## [[interface]] vs type

- [[타입스크립트(TypeScript)]]에서 type과 [[interface]]는 비슷한 역할을 하지만 사용성이 다르다.
- 타입 [[객체(Object)]]의 확장성을 위해서는 [[interface]]를 사용하는 것이 더 좋다.

### [[interface]] 

- [[extends]] [[키워드(Keyword)]]를 이용해서 확장할 수 있다.

```ts
interface Person {
	name: string;
	age: number;
}

interface Student extends Person { // 확장(상속)
	school: string;
}

const jieun: Student = {
  name: 'jieun',
  age: 27,
  school: 'HY'
}
```

- 또한 [[interface]]의 경우, [[선언적 확장(Declaration Merging)]]이 가능하다.  

### type

- & 기호를 이용해서 확장할 수 있다.

```ts
type Person = {
	name: string,
	age: number
}

type Student = Person & { // 확장(상속)
	school: string
}

const jieun: Student = {
	name: 'jieun',
	age: 27,
	school: 'HY'
}
```

- type의 경우 [[선언적 확장(Declaration Merging)]]이 불가능하다.

```tsx
type Person = {
  name: string;
  age: number;
}

type Person = { // ❗️Error: Duplicate identifier 'Person'.
  gender: string;
}
```
