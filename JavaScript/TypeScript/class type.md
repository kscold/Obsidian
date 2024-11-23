- class 타입은 [[타입스크립트(TypeScript)]]의 [[클래스(class)]]를 기반으로 한 [[객체(Object)]]이다.

- [[클래스(class)]]에서 정의된 [[속성(Property)]]과 [[메서드(Method)]]를 가지며, [[클래스(class)]]의 [[인스턴스(Instance)]]로 생성된다.


## 예시

```ts
class User {
	id: number;
	name: string;
	email: string;
	
	getFullName() {
		return `${this.name}`;     
	} 
} 

const classUser = new User(); 

classUser.id = 1; 
classUser.name = "John Doe"; 
classUser.email = "john@example.com";
```
