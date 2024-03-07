- [[함수(Function)]]의 한 종류로 지금 함수 끝나고 그 다음에 실행할 함수를 의미한다.

- [[함수(Function)]]의 [[매개변수(parameter)]]를 통해 다른 함수의 내부로 전달되는 함수를 콜백 함수라고 한다.
- 다른 함수(caller)의 전달 인자(argument)로 전달되는 함수를 콜백 함수라고 한다.
- 따라서 콜백 함수는 다른 함수의 인자로 전달되어 나중에 호출되는 함수를 말한다.

- 주로 [[화살표 함수(Arrow function)]]를 사용하여 표현한다.

- 주로 [[비동기(asynchronous)]] 작업에서 사용되며, 작업이 완료되었을 때 호출(비동기 [[JavaScript/고차함수(Higher Order Function)]]]되어 특정 동작이나 로직을 수행한다.

## 콜백 함수의 예시

- [[setTimeout()]]의 경우, 콜백 함수를 [[매개변수(parameter)]]로 가지는 대표적인 함수이다.

```js
funtion getData(callback) {
	setTimeoit(() => {
		console.log('서버에서 데이터를 받아왔어요');
		callback({ name: "별코딩" }); // 후처리가 가능하다.
	}, 2000);
}

getData((data) => { // 전달해준 콜백 함수가 getData(callback)의 인자로 들어감
	console.log(data.name);
})
```

- 밑의 코드는 콜백 함수가 같은 형식으로 중첩으로 되어있는 예시이다.

```jsx
setTimeout(() => { 
	setTimeout(() => { 
		console.log('todo: Second work!'); 
	}, 2000); 
	
	console.log('todo: First work!'); 
}, 3000);
```

- 중첩순차적으로 처리되지 못한 작업을 [[비동기(asynchronous)]]적인 작업이라고 하고 순차적으로 처리된 작업을 [[동기(Synchronous)]]적인 작업이라고 한다.

- 비동기 작업을 동기적으로 처리하기 위해 콜백 함수가 존재한다.