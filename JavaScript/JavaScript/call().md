- call() [[메서드(Method)]]는 [[함수(Function)]] [[객체(Object)]]에 미리 정의되어 있는 [[함수(Function)]]로 첫번째 인자로 [[객체(Object)]]를 주는데 call()을 호출하는 [[함수(Function)]]가 인자로 주어진 [[객체(Object)]]에 딸려 있는 [[객체(Object)]] 처럼 동작하게 한다.

- 비슷하게 사용되는데 인자가 [[배열(Array)]]로 들어가는 [[apply()]]가 있다.

- call() [[메서드(Method)]]를 사용하는 즉시 바로 호출이 된다.
- [[bind()]]의 경우는 call()과 거의 유사하나 바로 호출되지 않는다.

## 문법


```js
fun.call(thisArg[, arg1[, arg2[, ...]]])
```


## 예시

- 아래 코드와 같이 [[함수(Function)]]와 [[객체(Object)]]를 연결시킬 수 있다.

```js
const funllName = function (city, country) {
	console.log(this.firstName + ' ' + this.lastName);
};

const person1 = {
	firstName: 'John',
	lastName: 'Smith',
};

  
funllName.call(person1, 'Oslo', 'Norway');

// >> John Smith Oslo Norway
```