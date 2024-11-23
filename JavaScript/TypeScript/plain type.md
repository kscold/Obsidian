- `plain` 타입은 순수한 자바스크립트 [[객체(Object)]]를 나타낸다.
- [[객체(Object)]] [[리터럴(literal)]]이나 [[JSON(Java Script Object Notation)]] 형태로 나타내며, 특정 [[클래스(class)]]를 기반으로 생성되지 않는다.


## 예시

- plain 타입의 [[객체(Object)]]는 [[타입스크립트(TypeScript)]]의 타입 시스템에서 정적인 구조를 가지고 있으며, 동적으로 정의된 [[클래스(class)]]와 연결되지 않는다.

```js
const plainUser = {
	id: 1,
	name: "John Doe",
	email: "john@example.com",
	extraField: "someValue", // 클래스에 정의되지 않은 필드도 포함 가능 
};
```