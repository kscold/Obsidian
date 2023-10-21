
- 일급 함수란 함수를 일급 객체로 취급하는 것을 말한다. 
- 자바스크립트 또는 다른 함수형 프로그래밍 언어 함수들은 전부 객체(objects)이기 때문이다.
- 자바스크립트의 경우 함수도 객체로 표현하기 때문에 일급 객체 및 일급 함수라고 부른다. 

자바스크립트가 [[일급객체(First Class Object)]]이기 때문에 아래와 같은 것들을 할 수 있다.

> [[콜백 함수(callback)]]  
> [[고차함수(Higher-Order Function)]]
> **클로저(Closure)**


자바스크립트에서, 함수는 객체의 특별한 타입이다. 함수는 `Function` 객체이다.

예시

```js
function greeting() {
  console.log('Hello World');
}

// 함수 호출하기
greeting(); // prints 'Hello World'
```

밑에 코드는 자바스크립트에서 함수가 오브젝트인 것을 증명하기 위해한 예시

```js
// 우리는 오브젝트에 프로퍼티를 추가하듯 함수에 프로퍼티를 추가할 수 있습니다. 
greeting.lang = 'English'; // 객체에 추가하는 듯이 사용

// Prints 'English'
console.log(greeting.lang);
```