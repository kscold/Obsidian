- [[비동기(asynchronous)]] 처리에 사용되는 자바스크립트 [[객체(Object)]]이다.
- [[비동기(asynchronous)]] 작업이 맞이할 성공 혹은 실패를 나타낸다.

## Promise 문법

- Promise의 특징은 [[new]] Promise() [[메서드(Method)]]를 호출하는 동시에 바로 Promise 안에 내용이 호출되어 대기(Pending) 상태가 된다.

- 따라서 원하는 때에만 Promise를 실행시키고 싶다면 상위에 [[함수(Function)]]를 선언하여 Promise 전체를 넣어 주면 된다.
- 만약 함수 호출이라면 promise 객체를 return 해줘어야한다.

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
- 따라서 Promise는 [[then()]], [[catch()]], [[finally()]]라는 [[메서드(Method)]]를 제공한다.

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

### Promise chainning

```js
function getData() {
	const promise = new Promise((resolve, reject) => {
		setTimeout(() => {
			const data = { name: '철수' };
			// const data = null
			
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
const promise = getData();
promise.then((data) => {
	return getData()
}).then((data) => {
	return getData()
}).then((data) => {
	console.log(data);
})

// 바로 리턴
const promise = getData();
promise
	.then((data) => getData())
	.then((data) => getData())
	.then((data) => getData())

```

## Promise의 작동원리

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
- Promise 상태([[state]])가 Fulfilled(성공)일 때는 Result가 결과값(보통 JSON과 같은 데이터)가 된다.
- Promise 상태([[state]])가 Rejected(실패)일 때는 Result가 Error [[객체(Object)]]가 들어간다.