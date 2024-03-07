- 자바스크립트를 [[함수형 프로그래밍(Functional Programming)]]에 알맞은 언어로 만들어주는 특성이 바로 자바스크립트가 고차 함수 개념이 있다는 것이다.

- [[매개변수(parameter)]]를 통해 함수의 외부에서 [[콜백 함수(Callback Function)]]를 전달받은 함수를 고차 함수라고 한다.

- 고차 함수는 [[매개변수(parameter)]]를 통해 전달받은 [[콜백 함수(Callback Function)]]의 호출 시점을 결정해서 호출(invoke)한다.
- 즉, 콜백 함수는 고차 함수에 의해 호출되며 이때 고차 함수는 필요에 따라 콜백 함수에 인수를 전달할 수 있다.
- 따라서 고차 함수에 콜백 함수를 전달할 때 콜백 함수를 호출하지 않고 함수 자체를 전달해야 한다.

- 고차 함수라는 용어를 '함수를 전달인자(Argument)로 받는 함수'라고 사용할 경우 '커링 함수'라고도 한다. 
- 그러나, 커링 함수는 고차 함수에 포함되는 개념이다.

## 예시

```javascript
// 함수 funcA는 콜백 함수인 funcB를 외부에서 매개변수를 통해 전달받은 '고차 함수'임
function funcA(callback) {
	callback()
}

// 함수 funcB는 동기적으로 실행되고 있는 함수 funcA의 '콜백 함수'임
function funcB() {
	console.log('내가 콜백함수다!')
}

console.log(funcA(funcB))  
// >> 내가 콜백함수다!
```