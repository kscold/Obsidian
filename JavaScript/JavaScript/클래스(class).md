- 자바스크립트에서 클래스는 특정 [[객체(Object)]]를 생성하기 위해 [[변수(Variable)]]와 [[메서드(Method)]]드를 정의하는 일종의 틀로, [[객체(Object)]]를 정의하기 위한 상태([[state]])와 메서드([[함수(Function)]])로 구성된다.

- 자바스크립트는 사실 일반적인 클래스 개념이 아닌 프로토타입([[prototype]])기반의 언어이다.
- 따라서 [[prototype]]를 통해 초기 [[메서드(Method)]] 정의 및 오버라이딩이 가능하다.

- ES6부터 추가된 class [[키워드(Keyword)]]는 직관적으로 쉽게 코드를 읽을 수 있게 만들어 줄 뿐만 아니라, 작성하기도 쉽고 또 class 기반 언어에 익숙한 개발자가 더 빠르게 적응할 수 있다.
- 즉, 자바와 비슷하게 코딩이 가능해졌다.


## 클래스 사용 이유

- 클래스를 사용하지 않아도 [[new]] 연산자와 [[생성자(Constructor)]] 함수인 new function을 사용할 수 있다.
- 그러나 실무에선 사용자나 물건같이 동일한 종류의 [[객체(Object)]]를 여러 개 생성해야 하는 경우가 잦다.
- 따라서 클래스(class) 문법을 사용하면 객체 지향 프로그래밍에서 사용되는 다양한 기능을 자바스크립트에서도 사용할 수 있다.

- 밑의 예시는 클래스 [[키워드(Keyword)]]를 사용하지 않고 [[new]]를 이용해 [[객체(Object)]]를 만드는 코드이다.
- 즉, 클래스가 등장 이전에는 [[prototype]]를 사용하여 클래스처럼 동작하게 만들었다.

```javascript
function Me(name) { // 생성자 함수(function 키워드로만 만들어야함)
	this.name = name;
}

Me.prototype.wow = function () { // 메서드 처음 정의, 오버라이딩도 가능, 인스턴스 메서드임
	console.log("WOW!");
};

let person = new Me("Jason"); // new를 통해 새로운 객체를 생성

person.wow(); // WOW!
```

- 밑의 예시는 클래스 키워드를 사용한 코드이다.
- [[생성자(Constructor)]]인 [[constructor()]] 함수를 통해 [[this]]로 [[인스턴스(Instance)]] [[변수(Variable)]]을 만들어 줬다.

```javascript
class Me { // 자바와 비슷한 코딩이 가능해짐
	constructor(name){ // 생성자 메서드
		this.name = name;
	}
	  
	wow() { // 인스턴스 메서드
		console.log("WOW!");
	}

	static some() { // 정적 메서드도 선언가능
	...
	}
}
  
let person = new Me("Jason");
person.wow() // WOW!
```

## class 살펴보기

```javascript
class Korean {
	constructor(name, age) {
		this.name = name;
		this.age = age;
		this.country = 'Korea';
	}
	
	addAge(age) {  
		return this.age + age;
	}
}
```

- 자바스크립트에서도 class [[키워드(Keyword)]] 내에 정의된 [[함수(Function)]]를 [[메서드(Method)]]라고 부른다.
- 또한 class를 통해 생성된 [[객체(Object)]]를 [[인스턴스(Instance)]]라고 부른다.

- 클래스도 [[함수(Function)]]처럼 호출하기 전까지는 코드가 실행되지 않는다. 
- 우의 예시에서는 단지 Korean이라는 클래스를 정의만 했을 뿐이다.

- [[new]] [[키워드(Keyword)]]와 소괄호`()`를 사용하여 호출할 수 있다.

- 클래스 이름은 Korean과 같이 항상 대문자로 시작한다.

- [[constructor()]]는 class에서 필요한 기초 정보([[생성자 함수(Constructor Function)]])를 세팅하는 곳이다.

- [[객체(Object)]]를 [[new]]로 생성할 때 가장먼저 자동으로 호출된다.
    - [[constructor()]] [[메서드(Method)]]에서 `name`과 `age`, 2개의 [[매개변수(parameter)]]로 `Korean` [[인스턴스(Instance)]]의 `name`, `age` [[속성(Property)]]에 값을 할당했다.
    
    - [[this]] 는 본인 [[객체(Object)]]를 의미한다. 
    - 클래스 내에서 [[메서드(Method)]]끼리 소통하기 위해서는 [[this]]가 필요하다.

```js
let kim = new Korean("KIMJINYOUNG", 24);


// >> {
// >> 	name: 'KIMJINYOUNG',
// >> 	age: 24,
// >> 	country: 'Korea',
// >> 	addAge: function(age){
// >> 		return this.age + age;  	
// >> 	}
// >> }
```

- 따라서 위와 같이 Korean 클래스를 이용해 kim [[객체(Object)]]를 만들면 위와 같은 [[인스턴스(Instance)]]가 생성된다.


## 자바스크립트의 [[상속(Inheritance)]]

- [[클래스(Class)]] 문법 이전에 [[상속(Inheritance)]]을 하기 위해서는 밑의 코드처럼 [[prototype]]의 create() [[메서드(Method)]]를 이용해서 사용을 해야 했다.

```js
var Human = function(type) {
	this.type = type || 'human';
};

Human.isHuman = function(type) {
	return human instanceof Human;
};

Human.prototype.breathe = function() {
	alert('h-a-a-a-m');
}

var Zero = function(type, firstName, lastName){
	Human.apply(this, arguments);
	this.firstName = firstName;
	this.lastName = lastName;
}

Zero.prototype = Object.create(Human.prototype); // 상속관계 생성
Zero.prototype.constructor = Zero; // 상속하는 부분
Zero.prototype.sayName = function() {
	alert(this.firstName + ' ' + this.lastName);
};

var oldZero = new Zero('human', 'Zero', 'Cho');
Human.isHuman(oldZero); // true
```

- [[클래스(Class)]] 문법 이후는 더 직관적으로 지바와 비슷하게 extends [[키워드(Keyword)]]를 이용해서 [[상속(Inheritance)]] 관계 확인이 가능해졌고 훨신 적은 코드로 [[상속(Inheritance)]]이 가능해졌다.
- [[super()]]를 통해 부모 클래스의 [[생성자(Constructor)]]([[생성자 함수(Constructor Function)]])를 가져올 수 있다.

```js
class Human {
	constructor(type = 'human') {
		this.type = type;
	}
	
	static isHuman(human) { // 정적 메서드 선언
		return human instanceof Human;
	}
	
	breathe() { // 인스턴스 메서드 선언
		alert'h-a-a-a-m');
	}
};

class Zero extends Human {
	constructor(type, firstName, lastName){
		super(type);
		this.firstName = firstName;
		this.lastName = lastName;
	}
	
	sayName() {
		super.breathe();
		alert(`${this.firstName} ${this.lastName}`);
	}
}

const oldZero = new Zero('human', 'Zero', 'Cho');
```