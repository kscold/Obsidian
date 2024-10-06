- 모든 [[함수(Function)]]는 [[this]]를 수정하게 해주는 내장 [[메서드(Method)]] bind를 제공한다.

- [[call()]], [[apply()]]와 다르게 bind()는 따로 호출을 해주어야 사용이 가능하다.


## 문법

```js
let 변수 = 함수.bind(객체);
```

- [[함수(Function)]].bind([[객체(Object)]])는 [[함수(Function)]]처럼 호출 가능한 '특수 객체(exotic object)'를 반환한다.
- 이 객체를 호출하면 [[this]]가 [[객체(Object)]]로 고정된 [[함수(Function)]]가 반환된다.

- 따라서 대입된 [[변수(Variable)]]를 호출하면 [[this]]가 고정된 [[함수(Function)]]를 호출하는 것과 동일한 효과를 본다.

## 예시

- 아래 예시 `funcUser`에는 `this`가 `user`로 고정된 `func`이 할당된다.

```js
let user = {
	firstName: "John"
};

function func() {
	alert(this.firstName);
}

let funcUser = func.bind(user); // 인자 user가 그대로 func의 this로 전달된다.

funcUser(); // John
```

- 여기서 `func.bind(user)`는 `func`의 `this`를 `user`로 '바인딩한 변형’이라고 생각하시면 된다.
- 인자는 원본 함수 `func`에 그대로 전달된다.

```jsx
let user = {
	firstName: "John" 
};

function func(phrase) {
	alert(phrase + ', ' + this.firstName);
} // this를 user로 바인딩합니다.

let funcUser = func.bind(user);

funcUser("Hello"); // Hello, John (인수 "Hello"가 넘겨지고 this는 user로 고정됩니다.)
```

- 이제 [[객체(Object)]] [[메서드(Method)]]에 `bind`를 적용해 보자.

```js
let user = {
	firstName: "John", 
	sayHi() {
		alert(`Hello, ${this.firstName}!`);
	}
};

let sayHi = user.sayHi.bind(user); // 이제 객체 없이도 객체 메서드를 호출할 수 있음

sayHi(); // Hello, John!
setTimeout(sayHi, 1000); // Hello, John! 
// 1초 이내에 user 값이 변화해도 sayHi는 기존 값을 사용함

user = {
	sayHi() {
		alert("또 다른 사용자!");
	}
}; 
```

- 메서드 user.sayHi를 가져오고, 메서드에 `user`를 바인딩한다.
- sayHi는 이제 묶인(bound) [[함수(Function)]]가 되어 단독으로 호출할 수 있고 [[setTimeout()]]에 전달하여 호출할 수도 있다.
- 어떤 방식이든 컨택스트는 원하는 대로 고정된다.

- 아래 예시를 실행하면 인수는 그대로 전달되고 bind에 의해 [[this]]만 고정된 것을 확인할 수 있다.

```js

let user = {
	firstName: "John",
	say(phrase) {
		alert(`${phrase},${this.firstName}!`}
	}
};

let say = user.say.bind(user);
say("Hello"); // Hello, John (인수 "Hello"가 say로 전달되었습니다.)
say("Bye"); // Bye, John ("Bye"가 say로 전달되었습니다.)` 
```


## 대규모 바인딩

- bind()를 사용하여 [[메서드(Method)]] 전체 [[바인딩(binding)]]할 수 있다.
- [[객체(Object)]]에 복수의 메서드가 있고 이 메서드 전체를 전달하려 할 땐, 반복문을 사용해 [[메서드(Method)]]를 바인딩할 수 있다.

```js
for(let key in user) {
	if(typeof user[key] == 'function') {
		user[key] = user[key].bind(user);
	}
}
```

- lodash 라이브러리를 사용해도 대규모 바인딩을 할 수 있다.