- 자바스크립트에서 함수는 [[객체(Object)]]이고 거기다가 [[일급 객체(First Class Object)]]이다. 

- 따라서 자바스크립트에서는 함수를 인자(argument) 즉, [[고차함수(Higher Order Function)]]로도, 리턴 값으로도 사용할 수 있고 [[변수(Variable)]]에 함수를 넣을 수도 있다.

- 자바스크립트에서 함수는 [[참조 타입(Reference Type)]](주소)이다.

- [[익명 함수]]로 만들어서도 많이 사용한다.


## 함수 정의

- 함수는 [[function]] [[키워드(Keyword)]]를 사용하여 만들 수 있고, [[화살표 함수(Arrow function)]]를 사용하며 만들 수있다.
- 그러나 [[화살표 함수(Arrow function)]]가 [[function]] [[키워드(Keyword)]]를 완전히 대체할 수는 없다.
- 예를 들어 [[생성자 함수(Constructor Function)]]를 만들 때에는 [[function]] [[키워드(Keyword)]]로만 선언해야 한다.

```js
function a() { // 함수 정의
	...
}

const b = () => { // 화살표 함수 정의
	...
}
```

## 함수 호출

- 함수 호출을 할 때 템플릿 문자열(백틱)을 사용해서 호출 할 수도 있다.

```js
function a() {
	...
}

a(); // 일반적인 함수 호출
a``; // 템플릿 문자열을 이용한 함수 호출(최신문법)
```


## 함수 선언식과 함수 표현식의 차이

- 둘의 차이점은 함수 선언식은 [[호이스팅(variable hoisting)]]에 영향을 받지만 함수 선언식은 [[호이스팅(variable hoisting)]]에 영향을 받지 않게 된다.

```js
// 호이스팅이 되지 않음
alert(foo()); // 에러
var foo = funtion() { return 5; }

// 호이스팅이 됨
alert(foo()); // 5 
funtion foo() { return 5; }
```

### 함수 선언식

- 함수 선언은 함수를 만들고 이름을 지정하는 것이다.
- [[function]] [[키워드(Keyword)]] 다음에 함수 이름을 작성할 때 함수 이름을 선언한다.

```js
function funcDeclaration() {
	return 'A function declaration 함수 선언문';
}
```
### 함수 표현식

- 함수 표현식은 함수를 만들고 [[변수(Variable)]]에 할당하는 경우이다.
- 함수는 익명(주로 [[화살표 함수(Arrow function)]])으로 이름이 없다.

```js
funcExpression = function() {
	return 'A function expression 함수 표현식';
}
```



