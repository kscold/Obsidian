


- var 키워드는 Scope가 함수  단위이다.

```js
funtion myFunction() {
	var a = "hello";
	if (true) {
		var a = "bye";
		console.log(a); // bye
	}
	console.log(a); // bye
}

myFunction()
```

- 따라서 let 혹은 const는 스코프가 함수 단위가 아닌 블록 단위이므로, if 문 내부에서 선언한 a 값은 if 문 밖의 a 값을 변경하지 않는다.
- 추가적으로 ES6 문법에서 var를 사용할 일은 없다.

```js
funtion myFunction() {
	let a = 1;
	if (true) {
		let a = 2;
		
		console.log(a); // 2
	}
	console.log(a); // 1
}

myFunction()
```

- 따라서 let과 const는 밑에와 같이 중복 선언이 불가능하다.

```js
let a = 1;
let a = 2; // 오류 Uncaught SyntaxError: Identifier 'a' has already been declared

const b = 1;
b = 2; // Uncaught SyntaxError: Assignment to constant variable.
```