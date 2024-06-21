- [[비동기(asynchronous)]] 처리에 사용되는 자바스크립트 [[객체(Object)]]이다.
- [[비동기(asynchronous)]] 작업이 맞이할 성공 혹은 실패를 나타낸다.

- 프로미스와 [[콜백 함수(Callback Function)]]와의 가장 큰 차이점은 코드를 분리하여 원하는 때에 실행시킬 수 있다는 점이다.
- 또한 [[콜백 함수(Callback Function)]]을 사용할 때의 콜백 지옥을 방지할 수 있다.

- [[then()]]을 사용한 [[Promise chainning]]를 [[async await]]를 사용하여 더욱 간결하게 만들 수 있다.

## Promise의 동작과정

- Promise는 상자([[객체(Object)]])에 비유할 수 있다.
- Promise는 [[비동기(asynchronous)]] 작업이 시작될 때 이 상자도 생성이 되는데, 처음에는 비어있다가 [[비동기(asynchronous)]] 완료될 때 결과물로 상자가 채워지게 된다.

- 즉, Promise는 비동기 작업의 상태([[state]])를 나타낸다.

![[Pasted image 20240131165114.png]]

- 따라서 Promise는 [[비동기(asynchronous)]] 작업이 맞이할 성공 혹은 실패를 나타낸다.

![[Pasted image 20240131165408.png]]

- Promise [[객체(Object)]]는 아래 이미지와 같이 구성된다.

![[Pasted image 20240131165530.png]]

- Promise 상태([[state]])가 가질 수 있는 종류는 총 3가지이다.

![[Pasted image 20240131165727.png]]

- Promise 상태([[state]])가 Pending(대기)일 때는 Result가 undefined가 된다.
- Promise 상태([[state]])가 Fulfilled(성공)일 때는 Result가 결과값(보통 [[JSON(Java Script Object Notation)]]과 같은 데이터)가 된다.
- Promise 상태([[state]])가 Rejected(실패)일 때는 Result가 Error [[객체(Object)]]가 들어간다.

## Promise 문법

- Promise의 특징은 [[new]] Promise() [[메서드(Method)]]를 호출하는 동시에 바로 Promise 안에 내용이 호출되어 대기(Pending) 상태가 된다.

- 따라서 원하는 때에만 Promise를 실행시키고 싶다면 상위에 [[함수(Function)]]를 선언하여 Promise 전체를 넣어 주면 된다.
- 만약 함수 호출이라면 Promise [[객체(Object)]]를 return 해줘어야한다.

```js
new Promise(내용); // 바로 실행

function getData() { // 원하는 때에만 호출
	const promise = new Promise(내용);
	
	return promise
}
```

- [[new]] Promise() 메서드를 호출할 때 [[콜백 함수(Callback Function)]]를 선언할 수 있고, 콜백 함수의 인자는 resolve, reject이다.
- resolve는 [[비동기(asynchronous)]] 작업이 완료되었다고 알려주고 결과 값을 반환하는 함수이다.
- reject는 [[비동기(asynchronous)]] 작업이 실패했을 때 에러를 반환하는 함수이다.

```js
const promise = new Promise((resolve, reject) => {
  // ...
});

// 또는
const promise = new Promise(function(resolve, reject) {
  // ...
});
```

- 만약 [[비동기(asynchronous)]] 작업이 아니라 일반 [[동기(Synchronous)]] 작업을 사용하면 바로 호출된다.

```js
const promise = new Promise((resolve, reject) => {
	console.log("비동기 작업")
});

// >> 비동기 작업
```

## Promise의 데이터 처리

- 밑의 코드는 기본적인 Promise의 동작 예시이다.

```js
function getData() {
	const promise = new Promise((resolve, reject) => {
		setTimeout(() => {
			// const data = { name: '철수' };
			const data = null
			
			if(data) { // 성공했을 때
				console.log('네트워크 요청 성공');
				resolve(data)
			} else { // 실패했을 때
				reject(new Error("네트워크 문제"));
			}
			
		}, 1000);
	});
	
	return promise
}

const promise = getData();

setTimeout(() => {
	console.log(promise);
}, 2000);
```

- 그러나 위의 코드의 경우 데이터를 받아 왔을 때, 후처리가 어렵다.
- 따라서 Promise는 [[then()]], [[catch()]], [[finally()]]라는 [[메서드(Method)]]를 제공하여 [[Promise chainning]]을 할 수 있다.

- 여기서 또한 중요한 사실은 Promise 생성 후 [[콜백 함수(Callback Function)]] 실행까지는 [[동기(Synchronous)]]적이나,  [[then()]], [[catch()]], [[finally()]] 후처리 [[메서드(Method)]]를 만나는 순간 [[비동기(asynchronous)]] 함수가 된다.
- 즉, [[이벤트 루프(Event Loop)]]의 백그라운드([[Web API]]) 작업으로 넘어가는 시점인 것이다.

- 또한 [[setTimeout()]]와 같은 작업이 같이 [[테스크 큐(Task Queue)]]에 들어 있다면, 일반 [[비동기(asynchronous)]] [[함수(Function)]]보다 우선 순위가 높기 때문에 Promise가 먼저 실행된다.

```js
function getData() {
	const promise = new Promise((resolve, reject) => {
		setTimeout(() => {
			// const data = { name: '철수' };
			const data = null
			
			if(data) { // 성공했을 때
				console.log('네트워크 요청 성공');
				resolve(data)
			} else { // 실패했을 때
				reject(new Error('네트워크 문제'));
			}
			
		}, 1000);
	});
	
	return promise
}

// Promise chaining
// then(), catch(), finally()
getData().then((data) => {
	const name = data.name
	console.log(`${name}님 안녕하세요`);
}).catch(() => {
	console.log('멋지게 에러처리를 했어요');
}).finally(() => {
	console.log('마무리 작업');
})
```

## Promise 메서드

- Promise가 지원하는 [[메서드(Method)]]는 다음과 같다.

### [[Promise.all()]]

### [[Promise.allSettled()]]

### [[Promise.any()]]

### [[Promise.race()]]

