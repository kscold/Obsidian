- Promise.all [[메서드(Method)]] 사용하면 여러개의 [[Promise]]를 한번에 처리할 수 있다.

- Promise.all의 작업 중 하나라도 실패가 되면 [[Promise]] reject시켜버린다.
- 이때 결과는 [[배열(Array)]] 형태로 반환된다.

## Promise.all()의 동작 과정

- 아래와 같이 서로 영향을 주지 않는 독립적인 [[비동기(asynchronous)]] 함수 getName과 getTodo가 있다.

```js
function getName() {
	return new Promise((resolve, reject) => {
		setTimeout(() => { // 비동기 함수
			resolve('철수');
			// reject(new Error('에러: 이름이 없어요'));
		}, 1000)
	})
}

function getTodo() {
	return new Promise((resolve, reject) => {
		setTimeout(() => {
			resolve('밥먹기');
			// reject(new Error('에러: 할일이 없어요));
		}, 2000)
	})
}

getName()
getTodo()
```

- 위의 코드는 어떻게 보면 굉장히 손해인데, 왜냐하면 getName과 getTodo는 독립적인 [[비동기(asynchronous)]]함수 이기 때문에 밑에 이미지와 같이 [[동기(Synchronous)]]적으로 실행하는 것은 비효율적이다.

![[Pasted image 20240202021721.png]]

```js
const promise = Promise.all([getName(), getTodo()]); 

promise.then((data) => {
	console.log(data); // ['철수' '밥먹기']
});
```

![[Pasted image 20240202022302.png]]

- Promise.all의 특징은 여러개 중 하나라도 실패가 되면 Promise를 reject시켜버린다.
- 이때 에러 메세지는 가장먼저 실패한 메세지를 기준으로 한다.
- 즉, 위의 경우 [[catch()]] 블록이 바로 실행되게 된다.
