- 생성자 함수는 자바스크립트에서 [[객체(Object)]]를 생성하기 위해 사용되는 특수한 함수이다.
- 생성자 함수로 객체를 생성하기 위해서는 [[new]] 연산자를 호출한다.

- 생성자 함수명의 첫글자는 대문자로 시작한다.

## 생성자 함수 생성

- 다음 예제는 Date()라는 생성자 함수와 [[new]] 연산자를 사용하여 Date [[객체(Object)]]를 생성한다.

```js
var date = new Date();
```

- 자바스크립트는 Date [[생성자(Constructor)]] 이외에도 Array, Boolean, Error, Function, Number 등 다양한 내장 생성자를 제공한다.


## 생성자 함수 만들기

- 개발자가 직접 생성자 함수를 만들어야 하는 경우가 존재한다. 
- 밑의 예시는 UserInfo라는 생성자 함수를 구현한다.

```js
// 생성자 함수
function UserInfo() {
	this.name = 'Nick';
	this.age = 20;
	this.addresss = 'Busan';
}

// 객체 생성
let userInfo = new UserInfo();

console.log(userInfo);

// >> UserInfo {name: 'Nick', age: 20, addresss: 'Busan'}
```

- 생성자 함수는 [[화살표 함수(Arrow function)]]로 만들 수 없으며 오직, [[function]] [[키워드(Keyword)]]를 사용해야 합니다.
- 생성자 함수의 이름은 생성자 함수와 일반 함수를 구분하기 위해 첫 글자를 대문자로 시작하는 것이 좋습니다.

## 매개변수가 존재하는 생성자 함수

- Date() 생성자 함수처럼 매개변수가 존재하는 생성자 함수를 만들 수 있습니다.

```js
// 매개변수가 없는 Date 생성자 함수
var date1 = new Date();

// 1개의 매개변수가 존재하는 Date 생성자 함수
var date3 = new Date('2020-5-4');

// 5개의 매개변수 존재하는 Date 생성자 함수
var date2 = new Date(1999,10,25,2,50);
```

- 다음 예시는 3개의 매개변수(name, age, address)를 가지는 생성자 함수이다.

```js
function UserInfo(name, age, address) {
	this.name = name;
	this.age = age;
	this.addresss = address;
}

let userInfo = new UserInfo('홍길동', 20, '서울');

console.log(userInfo);

// >> UserInfo {name: '홍길동', age: 20, addresss: '서울'}
```

## 생성자 함수의 [[this]]

- [[this]]는 쉽게 말하자면 자기 자신으로 생성자 함수를 호출한 [[객체(Object)]]를 의미한다.

```js
function UserInfo() { // 생성자 함수 정의
	this.name = 'Bob';
}

let user = new User(); // 객체 생성하여 인스턴스 연결
```

### [[생성자(Constructor)]] vs [[객체(Object)]] [[리터럴(literal)]]

- [[생성자(Constructor)]]를 굳이 만들어야 하냐는 의문을 가질 수 있다.
- 밑의 코드처럼 객체 리터럴을 활용하여 객체를 생성할 수 있기 때문이다.

```js
let userInfo = {
	name: '홍길동',
	age: 20,
	addresss: '서울'
}
```

- 동일한 [[속성(Property)]](name, age, address)를 가지는 3개의 [[객체(Object)]]가 필요한 경우 다음 코드처럼 객체를 생성할 수 있다.

```js
let userInfo1 = {
	name: '홍길동',
	age: 20,
	addresss: '서울'
}

let userInfo2 = {
	name: '홍길동',
	age: 20,
	addresss: '서울'
}

let userInfo3 = {
	name: '홍길동',
	age: 20,
	addresss: '서울'
}
```

- 소스코드가 중복되어 가독성이 떨어진다.
- 다음 코드처럼 [[객체(Object)]] usrInfo2와 userInfo3을 userInfo1으로 초기화하면 코드가 심플해진다.
- 하지만, 치명적인 단점이 존재한다.

```js
let userInfo1 = {
	name: '홍길동',
	age: 20,
	addresss: '서울'
}

let userInfo2 = userInfo1;
let userInfo3 = userInfo1;

userInfo3.name = '마이콜';

console.log(userInfo1.name);
console.log(userInfo2.name);
console.log(userInfo3.name);

// >> 마이콜
// >> 마이콜
// >> 마이콜
```

- userInfo3.name을 변경했는데, userInfo1.name, userInfo2.name도 변경되는 문제가 발생한다.
- 각 [[객체(Object)]]가 독립적으로 동작하지 않고 동일한 [[객체(Object)]]를 가리키고 있기 때문이다.

- 다음 코드처럼 [[생성자(Constructor)]]를 사용하여 3개의 [[객체(Object)]]를 생성하고 userInfo3.name의 값을 변경해보면 [[생성자(Constructor)]]를 사용하여 생성된 [[객체(Object)]]는 독립적으로 동작하는 것을 확인할 수 있다.

```js
function UserInfo() {
	this.name = '홍길동';
	this.age = 20;
	this.addresss = '서울';
}

let userInfo1 = new UserInfo(); // 독립된 인스턴스 생성
let userInfo2 = new UserInfo();
let userInfo3 = new UserInfo();

userInfo3.name = '마이콜'

console.log(userInfo1.name);
console.log(userInfo2.name);
console.log(userInfo3.name);
// >> 홍길동
// >> 홍길동
// >> 마이콜
```

- 생성자 함수는 동일한 [[속성(Property)]]을 가지는 [[객체(Object)]]를 심플하게 생성할 수 있으며, 각 [[객체(Object)]]의 독립성을 보장한다.

## 객체 프로토타입

- 프로토타입([[prototype]])을 사용하여 [[속성(Property)]]과 [[함수(Function)]]를 추가할 수 있다.

```js
function UserInfo() { // 생성자 함수 정의
	this.name = '홍길동';
}

let userInfo1 = new UserInfo(); // 인스턴스 생성
let userInfo2 = new UserInfo();

UserInfo.prototype.age = 20; // 오버라이딩 가능

console.log(userInfo1.age);
console.log(userInfo2.age);

// >> 20
// >> 20
```

- UserInfo 생성자 함수에는 name 프로퍼티만 존재했는데, prototype을 사용하여 age 프로퍼티를 추가했다.

## 생성자 함수의 함수

- 생성자 함수에 [[함수(Function)]]가 존재할 수 있다.
- 다음 코드처럼 [[function]] [[키워드(Keyword)]]를 사용하여 함수를 정의한다.

```js
function UserInfo(name, age, address) {
	this.name = name;
	this.age = age;
	this.addresss = address;
	  
	this.info = function() { // 생성자 함수 내부의 함수
		return `${name}의 나이는 ${age}살이며, 주소는 ${address}입니다.`;
	}
}

let userInfo = new UserInfo('둘리', 20, '평양'); 
// 매개변수를 받는 생성자 함수 객체 생성 인스턴스 연결

console.log(userInfo.info());

// >> 둘리의 나이는 20살이며, 주소는 평양입니다.
```

- 함수를 호출하는 방법과 동일하다.

## 생성자 모드 테스트

- [[new]] 연산자 없이 생성자 함수를 호출하는 경우 new.target이라는 [[속성(Property)]]를 사용하여 new 연산자 사용 여부를 확인할 수 있다.

- new 연산자 사용 여부를 확인하기 위해 생성자 함수에 new.target [[속성(Property)]]을 추가하고 객체를 생성한다.

```js
function UserInfo(name, age, address) {
	this.name = name;
	this.age = age;
	this.addresss = address;
	
	this.info = function() {
		return `${name}의 나이는 ${age}살이며, 주소는 ${address}입니다.`;
	}
	
	console.log(new.target);
}

let userInfo = new UserInfo('둘리', 20, '평양');
```

![[Pasted image 20240310030355.png]]

- new 연산자를 사용하지 않고 생성자 함수를 호출하는 경우 new.target은 [[undefined]]이다.

```js
function UserInfo(name, age, address) {
	this.name = name;
	this.age = age;
	this.addresss = address;
	this.info = function() {
		return `${name}의 나이는 ${age}살이며, 주소는 ${address}입니다.`;
	}
	console.log(new.target);
}

let userInfo = UserInfo('둘리', 20, '평양');

// >> undefined
```

## 괄호 생략

- 인자가 없는 생성자 함수를 호출하는 경우 다음 코드처럼 괄호를 생략할 수 있다.

```js
function UserInfo() { // 매개 변수가 없는 생성자 함수
	this.name = '또치';
	this.age = 20;
}

let user = new UserInfo;

console.log(user);

// >> UserInfo {name: '또치', age: 20}
```

## 생성자 함수의 [[인스턴스(Instance)]]인지 확인

- [[객체(Object)]]가 무슨 타입인지 혹은 어떤 생성자 함수로 생성되었는지 확인하고 싶은 경우 [[instanceof]] 연산자를 사용할 수 있습니다.

- 다음 코드는 user라는 [[객체(Object)]]가 [[생성자(Constructor)]] UserInfo()에 의해 생성되었으므로 true가 반환된다.

```js
function UserInfo() {
	this.name = '또치';
	this.age = 20;
}

let user = new UserInfo(); // 객체 생성

console.log(user instanceof UserInfo); 
console.log(user instanceof Date);

// >> true
// >> false
```

## 생성자 함수에서 return문

- 생성자 함수도 함수이므로 일반 [[함수(Function)]]처럼 return문이 존재할 수 있다.
- 생성자 함수에 return문이 존재하는 경우 [[this]] 대신 return문에 존재하는 값이 반환된다.

- 그러므로 [[instanceof]] 연산자를 사용하여 [[객체(Object)]]의 타입을 확인하면 예상과 다른 결과를 확인할 수 있다.

- 다음 코드는 생성자 함수에 return문이 존재하는 경우이다.

```js
function UserInfo(name, age, address) {
	this.name = name;
	this.age = age;
	this.addresss = address;
	
	return {
		name: '또치',
		age: 10
	}
}

let userInfo = new UserInfo('둘리', 20, '서울');

console.log(userInfo);
console.log(userInfo instanceof UserInfo); // return 값의 객체가 반환되므로 instanceof가 false

// >> { name: '또치', age: 10 }
// >> false
```