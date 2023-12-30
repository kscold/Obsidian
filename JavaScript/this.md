- 자바스크립트 내에서 this는 '누가 나를 불렀느냐'를 한다.

- 즉, 선언이 아닌 호출에 따라 달라진다.


## 1. 단독으로 쓴 this

- 단독으로 this를 호출하는 경우엔 global object를 가리킨다.

- 브라우저에서 호출하는 경우 object Window가 되겠죠?

- 이는 ES5에서 추가된 strict mode(엄격 모드)에서도 마찬가지이다.

![](https://blog.kakaocdn.net/dn/duY8YT/btqDKUV2At7/shsc6qD3lLN9gpxQgeqKi0/img.png)

```javascript
'use strict'; // 엄격모드 사용
var x = this;
console.log(x); //Window
```

## 2. 함수 안에서 쓴 this

- 함수 안에서 this는 함수의 주인에게 바인딩된다.

- 함수의 주인은  [[window]] [[객체(Object)]]이다.

```javascript
function myFunction() { 
	return this;
}

console.log(myFunction()); // window
```

```javascript
var num = 0;

function addNum() {  
	this.num = 100;  
	num++;    
	
	console.log(num); // 101 
	console.log(window.num); // 101 
	console.log(num === window.num); // true
} 

addNum();
```

위 코드에서 this.num의 this는 window 객체를 가리킨다.

따라서 num은 전역 변수를 가리키게 된다.

다만, strict mode(엄격 모드)에서는 조금 다르다.

함수 내의 this에 디폴트 바인딩이 없기 때문에 [[undefined]]가 된다.

```javascript
"use strict"; 
function myFunction() { 
	return this;
}

console.log(myFunction()); //undefined
```

```javascript
"use strict"; 
var num = 0;

function addNum() { 
	this.num = 100; //ERROR! Cannot set property 'num' of undefined 
	num++;
} 
 
 addNum();
```

따라서 this.num을 호출하면 undefined.num을 호출하는 것과 마찬가지기 때문에 에러가 난다.

## 3. [[메서드(Method)]] 안에서 쓴 this

- 메서드 호출 시 메서드 내부 코드에서 사용된 this는 해당 메서드를 호출한 객체로 바인딩된다.

```javascript
var person = { firstName: 'John', lastName: 'Doe', fullName: function () {
	return this.firstName + ' ' + this.lastName; 
},}; 

person.fullName(); //"John Doe"
```

```javascript
var num = 0; function showNum() {  console.log(this.num);} showNum(); //0 var obj = {  num: 200,  func: showNum,}; obj.func(); //200
```

## 4. 이벤트 핸들러 안에서 쓴 this

이벤트 핸들러에서 this는 이벤트를 받는 HTML 요소를 가리킵니다.

```javascript
var btn = document.querySelector('#btn')btn.addEventListener('click', function () {  console.log(this); //#btn});
```

## 5. 생성자 안에서 쓴 this

생성자 함수가 생성하는 객체로 this가 바인딩 됩니다.

```javascript
function Person(name) {  this.name = name;} var kim = new Person('kim');var lee = new Person('lee'); console.log(kim.name); //kimconsole.log(lee.name); //lee
```

하지만 new 키워드를 빼먹는 순간 일반 함수 호출과 같아지기 때문에, 이 경우는 this가 window에 바인딩됩니다. (🐄오름)

```javascript
var name = 'window';function Person(name) {  this.name = name;} var kim = Person('kim'); console.log(window.name); //kim
```

## 7. 명시적 바인딩을 한 this

명시적 바인딩은 짝을 지어주는 거에요. 이 this는 내꺼! 같은 거 ;P

[apply()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) 와 [call()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function/call) 메서드는 Function Object에 기본적으로 정의된 메서드인데요, 인자를 this로 만들어주는 기능을 합니다.

```javascript
function whoisThis() {  console.log(this);} whoisThis(); //window var obj = {  x: 123,}; whoisThis.call(obj); //{x:123}
```

apply()에서 매개변수로 받은 첫 번째 값은 함수 내부에서 사용되는 this에 바인딩되고,

두 번째 값인 배열은 자신을 호출한 함수의 인자로 사용합니다. 

어떻게 활용하냐면요...

```javascript
function Character(name, level) {  this.name = name;  this.level = level;} function Player(name, level, job) {  this.name = name;  this.level = level;  this.job = job;}
```

이렇게 두 생성자 함수가 있다고 해봅시다. this.name과 this.level을 받아오는 부분이 똑같습니다.

이럴 때 apply()을 쓸 수 있어요.

```javascript
function Character(name, level) {  this.name = name;  this.level = level;} function Player(name, level, job) {  Character.apply(this, [name, level]);  this.job = job;} var me = new Player('Nana', 10, 'Magician');
```

call()도 apply()와 거의 같습니다.

차이점이 있다면 call()은 인수 목록을 받고 apply()는 인수 배열을 받는다는 차이가 있어요.

위 코드를 call()로 바꿔 쓴다면 이렇게 되겠죠 :)

둘다 일단은 함수를 호출한다는 것에 주의하세요.

```javascript
function Character(name, level) {  this.name = name;  this.level = level;} function Player(name, level, job) {  Character.call(this, name, level);  this.job = job;} var me = {};Player.call(me, 'nana', 10, 'Magician');
```

apply()나 call()은 보통 유사배열 객체에게 배열 메서드를 쓰고자 할 때 사용합니다.

예를 들어 arguments 객체는 함수에 전달된 인수를 Array 형태로 보여주지만 배열 메서드를 쓸 수가 없습니다.

이럴 때 쓱하고 가져다 쓸 수 있어요.

```javascript
function func(a, b, c) {  console.log(arguments);   arguments.push('hi!'); //ERROR! (arguments.push is not a function);}
```

```javascript
function func(a, b, c) {  var args = Array.prototype.slice.apply(arguments);  args.push('hi!');  console.dir(args);} func(1, 2, 3); // [ 1, 2, 3, 'hi!' ]
```

```javascript
var list = {  0: 'Kim',  1: 'Lee',  2: 'Park',  length: 3,}; Array.prototype.push.call(list, 'Choi');console.log(list);
```

+

추가로 ES6부터 [Array.from()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/from)이라는 메서드를 쓸 수 있어요.

유사배열객체를 얕게 복사해 새 Array 객체로 만듭니다.

```javascript
var children = document.body.children; // HTMLCollection children.forEach(function (el) {  el.classList.add('on'); //ERROR! (children.forEach is not a function)});
```

```javascript
var children = document.body.children; // HTMLCollection Array.from(children).forEach(function (el) {  el.classList.add('on'); });
```

## 6. 화살표 함수로 쓴 this

'으아니! 챠! 왜 함수 안에서 this가 전역 객체가 되는 거야!!' 싶을 땐 화살표 함수를 쓰면 됩니다.

화살표 함수는 전역 컨텍스트에서 실행되더라도 this를 새로 정의하지 않고, 바로 바깥 함수나 클래스의 this를 쓰거든요.

```javascript
var Person = function (name, age) {  this.name = name;  this.age = age;  this.say = function () {    console.log(this); // Person {name: "Nana", age: 28}     setTimeout(function () {      console.log(this); // Window      console.log(this.name + ' is ' + this.age + ' years old');    }, 100);  };};var me = new Person('Nana', 28); me.say(); //global is undefined years old
```

위 코드를 보면 내부 함수에서 this가 전역 객체를 가리키는 바람에 의도와는 다른 결과가 나왔습니다.

```javascript
var Person = function (name, age) {  this.name = name;  this.age = age;  this.say = function () {    console.log(this); // Person {name: "Nana", age: 28}     setTimeout(() => {      console.log(this); // Person {name: "Nana", age: 28}      console.log(this.name + ' is ' + this.age + ' years old');     }, 100);  };};var me = new Person('Nana', 28); //Nana is 28 years old
```

하지만 화살표 함수로 바꾸면 제대로 된 결과가 나오는 걸 볼 수 있습니다.