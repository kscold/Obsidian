-  자바스크립트에서 속성(Property)은 [[객체(Object)]] 내부의 속성을 의미한다.

- [[객체(Object)]]는 속성(Property)로 구성된다. 
- 프로퍼티는 `"key(키)" : "value(값)"` 의 형식으로 객체 안의 콤마(쉼표 **,**)로 구분되어 할당된다. 

- Key는 속성명이라 생각해도 되고 문자열만 가능하며, 문자열이지만 따옴표가 없어도 된다.
- Value는 속성값이라고도 부르며, 어떤 값이든지(문자열, 숫자, 객체, 함수 등 아무거나) 상관없다. 

- [[객체(Object)]]에 [[함수(Function)]]를 정의할 경우, 속성값이라 하지않고 [[메서드(Method)]]라고 부른다.  

- 객체를 활용함에 있어서 유용하다. 
- 특정 객체가 가지고 있는 정보를 품고 있기에 해당 객체가 가진 정보에 직접적으로 접근할 수 있게 해준다.

## 객체의 속성 접근

- 속성에 접근하는 방법은 2가지가 있다.
- 객체명은 Namespaces처럼 동작한다.

```js
변수명.a;    // 'a'
변수명['a']; // 'a'
```

```js
zero.firstName; // 'Zero' 
zero['firstName']; // 'Zero' 

zero.lastName; // 'Cho'
zero['lastName']; // 'Cho'
```

- 대괄호(브라캣 연산자)로 접근할 수 있다.
- 점 표기법을 이용한 접근할 수 있다.
    - 주로 마침표를 사용하고 대괄호는 속성명에 띄어쓰기가 들어가 있는 경우 사용한다.  
    - 간단한 Property일 수도 있고, 배열의 일부이거나 객체의 메서드를 호출할 수도 있다.

### 하위 Namespaces

- 다른 [[객체(Object)]]를 [[객체(Object)]] 멤버의 값으로 갖는 것도 가능하다.

```js
var obj = {
  a : {
    bb: 'bbb',
    cc: 'ccc'
  }
}

obj.a.bb; // "bbb"
obj.a.cc; // "ccc"
```

### 객체 속성 추가 및 변경

```jsx
// 원 객체
var zero = {
  firstName: 'Zero',
  lastName: 'Cho'
};
```

- 밑의 코드는 객체 속성값을 변경하는 예시이다.

```jsx
zero.lastName = 'Lee'; // lastName 속성값 변경
zero; // { firstName: 'Zero', lastName: 'Lee' }
```

```jsx
var zero = {
  body: {
    height: 173,
    weight: 66
  }
};
zero.body.height; // 173
```

### 객체의 key와 value 값을 이용한 접근

```jsx
const name = 'variantKey'
const object = {
	[name]: 'value' // variantKey : value로 설정된다.
};
```

### 객체의 속성 삭제

- 객체의 속성을 삭제하는 방법도 있다. 
- 앞에 delete [[키워드(Keyword)]]를 붙이면 된다.

```js
var obj = {
  a : {
    bb: 'bbb',
    cc: 'ccc'
  }
}

delete obj.a.bb;
obj.a; // { cc: 'ccc' }
```

```jsx
delete zero.body.height;
zero.body; // { weight: 66 }
```

### [[new]]를 이용한 객체 복사 생성

```jsx
var zero = new Object();
zero.firstName = 'Zero';
zero.lastName = 'Cho';

zero.body = new Object();
zero.body.height = 173;
zero.body.weight = 66;
```

## 객체와 반복문

- [[배열(Array)]]에 있는 값을 가져올 때, 객체의 속성들을 반복하여 작업할 때 사용한다.

```js
var obj = {'prop1': 100, 'prop2': 200, 'prop3': 300 } // 변수에 객체를 만듬.
for (key in obj) {
  console.log("key : " + key + ", value : " + obj[key]);
}

// 결과
// key : prop1, value : 100
// key : prop2, value : 200
// key : prop3, value : 300
```

## 속성의 종류

- [[함수(Function)]] 속성에는 기본적으로 arguments, caller, length, name, [[prototype]] 속성이 있다.

- arguments 속성은 함수가 호출될 때 전달된 인자를 담은 [[배열(Array)]]이다.
- caller는 함수를 호출한 [[함수(Function)]]이다.
- length는 함수의 [[매개변수(parameter)]]의 개수를 의미한다.
- prototype은 함수 [[객체(Object)]]에만 있고, [[생성자(Constructor)]] 함수로 객체를 생성할 때 사용한다.

## 함수 속성

 - [[함수(Function)]]도 [[객체(Object)]]이므로 속성을 가지게 된다.

```js
function sum (a, b){
	return a + b;
}

console.dir(sum)
```

![[Pasted image 20240215192907.png]]

 - sum 함수 생성 코드를 보면 argument, caller 등과 같은 속성들을 생성한 적이 없는데, 자동으로 생성이 되어있다.

- 이는 [[function]] [[생성자(Constructor)]]에 의해서 함수가 생성될 때 자동으로 할당이 된다.
- 함수에는 기본적으로 arguments, caller, length, name, prototype 등의 속성이 있다.

### arguments 속성

 - 함수 호출 시 전달되는 인자들의 정보를 담고 있는 [[배열(Array)]]이다.
 - 함수 안에서만 사용 가능한 지역 변수이다.

```js
function minus(a, b){
	console.log(arguments);
    return a - b;
}

minus(); // Arguments [callee: ƒ, Symbol(Symbol.iterator): ƒ]
minus(1); // Arguments [1, callee: ƒ, Symbol(Symbol.iterator): ƒ]
minus(2, 1); // Arguments(2) [2, 1, callee: ƒ, Symbol(Symbol.iterator): ƒ]
minus(3, 2, 1); // Arguments(3) [3, 2, 1, callee: ƒ, Symbol(Symbol.iterator): ƒ]
```

![](https://blog.kakaocdn.net/dn/Brwfs/btrsSjMrBCC/kNfNqxs2dniT2g3r7G9800/img.png)

- minus 함수에 3,2,1 인수를 전달했을 때의 arguments 객체이다.

 - arguments 프로퍼티는 일부 브라우저에서 지원은 하고 있으나, deprscated(중요도가 떨어져 더 이상 사용되지 않고 앞으로 사라지게 될)되어 실무에서는 거의 사용되고 있지 않다.

### caller 속성

  - caller 속성은 자신을 호출한 함수를 의미한다.

```js
function sum (a, b){
	console.log(sum.caller);
    
    return a+b;
}

function calculator(x, y) {
	let sum_ = sum(x, y);
    
    return sum_;
}

calculator(1, 10); // f calculator(x,y) {...}
sum(1,2); // null 브라우저에서 실행했으므로 caller가 없음
```

- calculator를 통해서 sum함수를 호출 시 sum의 caller에는 calculator가 들어가 있다.

- sum 함수를 직접 호출 시 caller는 null이다.
- 함수를 통해서 호출한 함수가 없기 때문이다.

### length 속성

 - length 프로퍼티는 함수 정의 시 작성된 [[매개변수(parameter)]] 개수를 의미한다.

```js
// 매개변수 0개
function func1(){}
// 매개변수 1개
function func2(a){}
// 매개변수 2개
function func3(a,b){}

console.log(func1.length); // 0
console.log(func2.length); // 1
console.log(func3.length); // 2
```

### name 속성

 - 함수명을 의미한다. 
 - 기명 함수(이름을 명시한 함수)의 경우 함수명을 값으로 갖고 익명 함수인 경우 빈 문자열을 값으로 갖게 된다.
 - 단, ES6이후부터는 익명([[화살표 함수(Arrow function)]] 혹은 [[function]] [[키워드(Keyword)]] 사용) 함수의 이름이 name의 값으로 할당된다.

```js
// 기명 함수
function sum(a, b) {
	return a+b;
}

// 익명 함수
let minus = function(a, b) {
	return a - b;
}

console.log(sum.name); // sum
console.log(minus.name); // ES6 이전 -> '' , ES6부터 -> minus
```

### `__proto__`접근자 속성

 - 모든 객체는 [[Prototype]]이라는 내부 슬롯이 있고, 프로토타입 객체를 가리킨다. 

 - 프로토타입 객체란 객체 간의 상속(Inheritance)을 구현하기 위해 사용된다. 
 - 여기서 상속은 쉽게 말해, 부모님이 자식에게 재산을 상속하듯이 부모 객체가 자식 객체에게 부모의 속성을 주는 것을 의미한다.

- __proto__ 프로퍼티는 [[Prototype]] 내부 슬롯이 가리키는 프로토타입 객체에 접근하는 접근 프로퍼티이다.
- 내부 슬롯에는 직접 접근을 할 수 없으므로 `__proto__`를 사용한다.

```js
console.log(Object.prototype) // { constructor: ƒ, __defineGetter__: ƒ, __defineSetter__: ƒ, hasOwnProperty: ƒ, __lookupGetter__: ƒ, …}

let obj = {};

console.log(obj.__proto__); // {constructor: ƒ, __defineGetter__: ƒ, __defineSetter__: ƒ, hasOwnProperty: ƒ, __lookupGetter__: ƒ, …}

console.log(Object.prototype === obj.__proto__); // true
```

- `__proto__` 프로퍼티는 객체가 직접 소유한 것이 아니고 Object.prototype 객체의 프로퍼티를 상속받은 것이다.

```js
// Object.prototype 객체에 새로운 프로퍼티 추가
Object.prototype.hello = function(){
	console.log('Hi');
}

let a = {};

console.log(a.__proto__.hello); //ƒ (){console.log('Hi');}
```

### prototype 속성

 - prototype 프로퍼티는 함수 객체에만 있는 속성이다. 
 - 일반 [[객체(Object)]]에는 prototype 속성이 없다.

```js
let a = {};

let b = function(){};

console.log(a.prototype); // undefined
console.log(b.prototype); // {constructor: ƒ}
```

- prototype 속성은 [[생성자 함수(Constructor Function)]]에 의해서 [[함수(Function)]]를 생성할 때 사용된다.

```js
function Puppy(name) {
	this.name = name;
}

Puppy.prototype = {
	귀여움 : true
}

let 꾸기 = new Puppy('꾸기'); // new 생성자 사용

console.log(꾸기); // Puppy {name: '꾸기'}
console.log(꾸기.귀여움); // true
```

