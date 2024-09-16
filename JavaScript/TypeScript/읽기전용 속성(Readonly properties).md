- [[객체(Object)]]가 처음 생성될 때만 값 설정이 가능하고, 이후 수정이 불가능하다.
- [[속성(Property)]] 이름 앞에 [[readonly]] [[키워드(Keyword)]]를 붙여 사용한다.


## 예시

```ts
interface Student {
	readonly studentNo: number;
	name: string;
}

let StudentInfo: Student = {
	studentNo: 123456,
	name: 'Cine',
};

StudnetInfo.studentNo = 111111; // Cannot addign to 'studentNo' because it is a read-only p
```