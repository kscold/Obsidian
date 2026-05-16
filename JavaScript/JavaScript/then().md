- then()은 [[Promise]] [[객체(Object)]]의 [[메서드(Method)]]로, Promise가 `fulfilled` 상태가 되었을 때 실행할 [[콜백 함수(Callback Function)]]를 등록한다.
- 즉, then() 자체가 "기다리는" 것이 아니라, **Promise가 완료되면 콜백을 실행하라**고 등록만 해두는 것이다.

- 등록된 [[콜백 함수(Callback Function)]]는 Promise가 `resolve`되는 시점에 **마이크로태스크 큐**(Microtask Queue)에 들어가고, [[호출 스택(Call Stack)]]이 비었을 때 [[이벤트 루프(Event Loop)]]가 꺼내 실행한다.

- 따라서 then()의 콜백은 **항상 비동기적으로** 실행된다. 동기 코드(아래 예시의 `console.log('after then')`)가 모두 끝난 다음에야 실행된다는 뜻이다.

```js
console.log('start');

Promise.resolve('hello').then((v) => {
	console.log(v); // 'hello'
});

console.log('end');

// 출력 순서: start → end → hello
```

- then()은 **새로운 [[Promise]] 객체를 반환**하므로 체이닝(`.then().then()`)이 가능하다.
- 콜백이 일반 값을 return하면 그 값으로 resolve된 Promise가, Promise를 return하면 그 Promise를 따라가는 Promise가, throw를 하면 rejected 상태의 Promise가 반환된다.


## 주의: then 콜백은 항상 마이크로태스크 큐

- [[setTimeout()]]/`setInterval`/[[이벤트(event)]] 콜백 등은 [[Web API]]가 처리한 뒤 콜백을 **매크로태스크 큐**(Task Queue)에 넣는다.
- [[Promise]]의 `.then`/`.catch`/`.finally` 콜백은 Promise가 settle되는 시점에 곧장 **마이크로태스크 큐**(Microtask Queue)로 들어간다.
- [[fetch()]]는 [[Promise]]를 반환하는 함수이며, 네트워크 I/O는 [[Web API]]가 처리하지만 **그 결과를 받는 `.then()` 콜백은 마이크로태스크 큐**로 들어간다(매크로태스크 큐가 아니다).
- [[이벤트 루프(Event Loop)]]는 [[호출 스택(Call Stack)]]이 비면 **마이크로태스크 큐를 먼저 모두 비운 뒤** 매크로태스크 큐에서 하나를 꺼낸다.
- 그래서 같은 tick에서 [[setTimeout()]] 0ms보다 then() 콜백이 먼저 실행된다.

```js
setTimeout(() => console.log('timeout'), 0);      // 매크로태스크
Promise.resolve().then(() => console.log('then')); // 마이크로태스크

// 출력: then → timeout
```
