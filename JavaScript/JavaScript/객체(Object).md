- 객체란, 현실의 사물을 코딩을 통해 컴퓨터에게 설명하기 위해 만든 덩어리이다.

- 자바스크립트에서는 기본 데이터 타입인 boolean, number, string, 그리고 특별한 값인 null, [[undefined]] 빼고는 모두 객체이다.
- 따라서 [[함수(Function)]]도 객체이며 이 때문에 [[일급 함수(First Class Function)]]와 [[일급 객체(First Class Object)]]의 특징을 가진다.

- `{}`를 사용해서 [[객체(Object)]]를 만드는 것이 더 일반적이며 이를 객체 [[리터럴(literal)]]이라고 한다.

- 객체 하나만 사용하는 것이 아니라 [[배열(Array)]]을 통한 또 하나의 덩어리를 통해 접근하여 사용하기도 한다.

- 참고로 객체에는 .length() [[메서드(Method)]]를 사용할 수 없다.

- 또란 객체 안에 선언된 [[메서드(Method)]]에서 [[this]]는 객체의 [[속성(Property)]]을 가리킬 수 있다.


## 객체 문법

- 객체의 key와 value의 형태의 딕셔너리를 [[속성(Property)]]이라고 한다.

```jsx
var zero = {
	firstName: 'Zero',
	lastName: 'Cho'
};
```

- 따라서 zero 객체에는 `firstName: 'Zero'`와 `lastName: 'Cho'`까지 두 개의 [[속성(Property)]]이 있다.
- [[속성(Property)]]끼리는 쉼표로 구분해준다. 

- 자바스크립트는 [[클래스(Class)]] 내부에 정의 된 [[함수(Function)]]를 [[메서드(Method)]]라고 부르는 것 뿐만 아니라 [[속성(Property)]]값이 [[함수(Function)]]인 것을 [[메서드(Method)]]라고 부른다.


## 객체 [[리터럴(literal)]]

- ES6 문법부터 객체의 [[메서드(Method)]]에 :funtcion을 붙이지 않아도 되게 바뀌었다.
- 또한 shorthand property를 사용하여 { sayNode: sayNode }를 { sayNode }로 축약 가능해졌다.
- `[`[[변수(Variable)]] + 값`]` 등의 형식으로 통해 [[속성(Property)]]명을 객체 [[속성(Property)]] 명으로 사용가능해졌다.

```js
const sayNode = {
	console.log('Node');
}

const newObject = {
	
	sayJS() {
		console.log('JS');
	},
	
	sayNode,
	[es + 6]: 'Fantastic',
};

newObject.sayNode(); // Node
newObject.sayJS(); // JS
console.log(newObject.ES6); // Fantasitc
```

## 객체 [[메서드(Method)]]

```js
const user = {
	name: "김승찬",
	age: 25,
	greet() {
		console.log("Hello!");
		console.log(this.age); // this.age는 25와 연결됨
	}
};

console.log(user.name);
user.greet()
```