- 일급 함수란 함수를 [[일급 객체(First Class Object)]]로 취급하는 것을 말한다. 
- 자바스크립트 또는 다른 [[함수형 프로그래밍(Functional Programming)]]언어 함수들은 전부 [[객체(Object)]]이기 때문이다.
- 따라서 일급 객체 및 일급 함수라고 부른다. 

- 자바스크립트에서, 함수는 객체의 특별한 타입이다. 
- 함수는 `Function` 객체이다.

- 자바스크립트가 [[일급 객체(First Class Object)]]이기 때문에 아래와 같은 것들을 할 수 있다.
	- [[콜백 함수(Callback Function)]]  
	- [[JavaScript/고차함수(Higher Order Function)]]
	- 클로저(Closure)

## 예시

- 밑에 코드는 자바스크립트에서 함수가 객체인 것을 증명하기 위해한 예시이다.
- 
```js
function greeting() {
  console.log('Hello World');
}

// 함수 호출하기
greeting(); // 'Hello World'이 출력
```

```js
greeting.lang = 'English'; // 객체에 추가하는 듯이 사용가능 함


console.log(greeting.lang); // 'English' 출력
``` 

- 위의 예시를 통해 자바스크립트에서 객체를 [[속성(Property)]]를 추가하듯 [[함수(Function)]]에 속성을 추가할 수 있다.