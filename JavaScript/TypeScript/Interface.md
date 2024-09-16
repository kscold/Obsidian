- 인터페이스(interface)는 일반적으로 타입 체크를 위해 사용되며 [[변수(Variable)]], [[함수(Function)]], [[클래스(class)]]에 사용할 수 있다. 

- 인터페이스는 여러가지 타입을 갖는 [[속성(Property)]]로 이루어진 새로운 타입을 정의하는 것과 유사하다. 
- 인터페이스에 선언된 [[속성(Property)]] 또는 [[메서드(Method)]]의 구현을 강제하여 일관성을 유지할 수 있도록 하는 것이다.

- ES6([[ES module]])는 인터페이스를 지원하지 않지만 [[타입스크립트(TypeScript)]]는 인터페이스를 지원한다.

- 인터페이스는 [[속성(Property)]]와 [[메서드(Method)]] 가질 수 있다는 점에서 클래스와 유사하나 직접 [[인스턴스(Instance)]]를 생성할 수 없고 모든 [[메서드(Method)]]는 추상 메서드(Abstract Method)이다. 
- 단, [[추상 클래스(Abstract Class)]]의 추상 메서드와 달리 인터페이스는 abstract 키워드를 사용하지 않는다.

- 따라서, interface로 정의한 값들은 모두 필수적으로 들어가야 하며, 하나라도 빠질 경우 [[에러(error)]]를 반환하고, [[타입 표기(Type Annotation)]]으로 지정한 [[메서드(Method)]] 모두 내부에서 재정의가 필요하다.


## [[변수(Variable)]]와 인터페이스

- 인터페이스는 변수의 타입으로 사용할 수 있다.
- 이때 인터페이스를 타입으로 선언한 변수는 해당 인터페이스를 준수하여야 한다.
- 이것은 새로운 타입을 정의하는 것과 유사하다.

```ts
// 인터페이스의 정의
interface Todo {
	id: number;
	content: string;
	completed: boolean;
}

// 변수 todo의 타입으로 Todo 인터페이스를 선언
let todo: Todo;

// 변수 todo는 Todo 인터페이스를 준수하여야 함
todo = { id: 1, content: 'typescript', completed: false };
```

- 인터페이스를 사용하여 [[함수(Function)]] [[매개변수(parameter)]]의 타입을 선언할 수 있다.
- 이때 해당 함수에는 [[함수(Function)]] [[매개변수(parameter)]]의 타입으로 지정한 인터페이스를 준수하는 인자를 전달하여야 한다.
- [[함수(Function)]]에 [[객체(Object)]]를 전달할 때 복잡한 [[매개변수(parameter)]] 체크가 필요없어서 매우 유용하다.

```ts
// 인터페이스의 정의
interface Todo {  
	id: number;
	content: string;
	completed: boolean;
}

let todos: Todo[] = [];

// 파라미터 todo의 타입으로 Todo 인터페이스를 선언
function addTodo(todo: Todo) {
  todos = [...todos, todo];
}

// 파라미터 todo는 Todo 인터페이스를 준수하여야 함
const newTodo: Todo = { id: 1, content: 'typescript', completed: false };
addTodo(newTodo);

console.log(todos)
// [ { id: 1, content: 'typescript', completed: false } ]
```


## [[함수(Function)]]와 인터페이스

- 인터페이스는 [[함수(Function)]]의 타입으로 사용할 수 있다. 
- 이때 [[함수(Function)]]의 인터페이스에는 타입이 선언된 [[매개변수(parameter)]] 리스트와 리턴 타입을 정의한다. 
- [[함수(Function)]] 인테페이스를 구현하는 함수는 인터페이스를 준수하여야 한다.

```typescript
// 함수 인터페이스의 정의
interface SquareFunc {
  (num: number): number;
}

// 함수 인테페이스를 구현하는 함수는 인터페이스를 준수하여야 한다.
const squareFunc: SquareFunc = function (num: number) {
  return num * num;
}

console.log(squareFunc(10)); // 100
```


## [[클래스(class)]]와 인터페이스

- 클래스 선언문의 [[implements]] 뒤에 인터페이스를 선언하면 해당 클래스는 지정된 인터페이스를 반드시 구현하여야 한다
- 이는 인터페이스를 구현하는 클래스의 일관성을 유지할 수 있는 장점을 갖는다. 
- 인터페이스는 [[속성(Property)]]와 [[메서드(Method)]]를 가질 수 있다는 점에서 [[클래스(class)]]와 유사하나 직접 [[인스턴스(Instance)]]를 생성할 수는 없다.

```ts
// 인터페이스의 정의
interface ITodo {
	id: number;
	content: string;
	completed: boolean;
}

// Todo 클래스는 ITodo 인터페이스를 구현하여야 함
class Todo implements ITodo {
	constructor (
	    public id: number,
	    public content: string,
	    public completed: boolean
	) {}
}

const todo = new Todo(1, 'Typescript', false);

console.log(todo);
```

- 인터페이스는 [[속성(Property)]]뿐만 아니라 [[메서드(Method)]]도 포함할 수 있다.
- 단, 모든 [[메서드(Method)]]는 [[추상 메서드(Abstract Method)]]이어야 한다.
- 인터페이스를 구현하는 [[클래스(class)]]는 인터페이스에서 정의한 [[속성(Property)]]와 [[추상 메서드(Abstract Method)]]를 반드시 구현하여야 한다.

```ts
// 인터페이스의 정의
interface IPerson {
	name: string;
	sayHello(): void;
}

/*
인터페이스를 구현하는 클래스는 인터페이스에서 정의한 프로퍼티와 추상 메소드를 반드시 구현하여야 함
*/
class Person implements IPerson {
	// 인터페이스에서 정의한 프로퍼티의 구현
	constructor(public name: string) {}
	
	// 인터페이스에서 정의한 추상 메소드의 구현
	sayHello() {
	    console.log(`Hello ${this.name}`);
	}
}

function greeter(person: IPerson): void {
	person.sayHello();
}

const me = new Person('Lee');
greeter(me); // Hello Lee
```


## [[덕 타이핑(Duck typing)]]

- 주의해야 할 것은 인터페이스를 구현하였다는 것만이 타입 체크를 통과하는 유일한 방법은 아니다.
- 타입 체크에서 중요한 것은 값을 실제로 가지고 있는 것이다.
- 이해가 어려울 수 있으므로 예를 들어 설명한다.

```ts
interface IDuck { // 1
	quack(): void;
}

class MallardDuck implements IDuck { // 3
	quack() {
	    console.log('Quack!');
	}
}

class RedheadDuck { // 4
	quack() {
	    console.log('q~uack!');
	}
}

function makeNoise(duck: IDuck): void { // 2
	duck.quack();
}

makeNoise(new MallardDuck()); // Quack!
makeNoise(new RedheadDuck()); // q~uack! // 5
```

1. 인터페이스 IDuck은 quack 메소드를 정의하였다.
2. makeNoise [[함수(Function)]]는 인터페이스 IDuck을 구현한 [[클래스(class)]]의 [[인스턴스(Instance)]] duck을 인자로 전달받는다.
3. [[클래스(class)]] MallardDuck은 인터페이스 IDuck을 구현하였다. 
4. [[클래스(class)]] RedheadDuck은 인터페이스 IDuck을 구현하지는 않았지만 quack [[메서드(Method)]]를 갖는다.
5. makeNoise 함수에 인터페이스 IDuck을 구현하지 않은 클래스 RedheadDuck의 [[인스턴스(Instance)]]를 인자로 전달하여도 에러 없이 처리된다.

- 인터페이스를 [[변수(Variable)]]에 사용할 경우에도 덕 타이핑은 적용된다.

```ts
interface IPerson {
	name: string;
}

function sayHello(person: IPerson): void {
	console.log(`Hello ${person.name}`);
}

const me = { name: 'Lee', age: 18 };
sayHello(me); // Hello Lee
```

- [[변수(Variable)]] me는 인터페이스 IPerson과 일치하지는 않는다.
- 하지만 IPerson의 name [[속성(Property)]]를 가지고 있으면 인터페이스에 부합하는 것으로 인정된다.

- 인터페이스는 개발 단계에서 도움을 주기 위해 제공되는 기능으로 자바스크립트의 표준이 아니다. 
- 따라서 위 예시의 [[타입스크립트(TypeScript)]] 파일을 자바스크립트 파일로 트랜스파일링하면 아래와 같이 인터페이스가 삭제된다.

```js
function sayHello(person) {
	console.log("Hello " + person.name);
}

var me = { name: 'Lee', age: 18 };
sayHello(me); // Hello Lee
```


## [[선택적 속성(Optional Properties)]]

- 인터페이스의 [[속성(Property)]]는 반드시 구현되어야 한다. 
- 하지만 인터페이스의 프로퍼티가 선택적으로 필요한 경우가 있을 수 있다. [[선택적 속성(Optional Properties)]]는 [[속성(Property)]]명 뒤에 ?를 붙이며 생략하여도 에러가 발생하지 않는다.

```ts
interface UserInfo {
	username: string;
	password: string;
	age?    : number;
	address?: string;
}

const userInfo: UserInfo = {
	username: 'ungmo2@gmail.com',
	password: '123456'
}

console.log(userInfo);
```

- 이렇게 [[선택적 속성(Optional Properties)]]를 사용하면 사용 가능한 [[속성(Property)]]를 파악할 수 있어서 코드를 이해하기 쉬워진다.

## 인터페이스 상속

- 인터페이스는 [[extends]] 키워드를 사용하여 인터페이스 또는 [[클래스(class)]]를 [[상속(Inheritance)]]받을 수 있다.

```ts
interface Person {
	name: string;
	age?: number;
}

interface Student extends Person {
	grade: number;
}

const student: Student =  {
	name: 'Lee',
	age: 20,
	grade: 3
}
```

- 복수의 인터페이스를 상속받을 수도 있다.

```ts
interface Person {
	name: string;
	age?: number;
}

interface Developer {
	skills: string[];
}

interface WebDeveloper extends Person, Developer {}

const webDeveloper: WebDeveloper =  {
	name: 'Lee',
	age: 20,
	skills: ['HTML', 'CSS', 'JavaScript']
}
```

- 인터페이스는 인터페이스 뿐만 아니라 클래스도 상속받을 수 있다.
- 단, 클래스의 모든 멤버([[public]], [[protected]], [[private]])가 상속되지만 구현까지 [[상속(Inheritance)]]하지는 않는다.

```ts
class Person {
	constructor(public name: string, public age: number) {}
}

interface Developer extends Person {
	skills: string[];
}

const developer: Developer =  {
	name: 'Lee',
	age: 20,
	skills: ['HTML', 'CSS', 'JavaScript']
}
```