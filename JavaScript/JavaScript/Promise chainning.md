- [[Promise]]로 반환되는 데이터들을 [[then()]], [[catch()]], [[finally()]] [[메서드(Method)]]로 꼬리를 물며 [[비동기(asynchronous)]] 작업들의 데이터를 [[동기(Synchronous)]]적으로 받아오는 것이다.
- 마치 고리가 연결되는 것을 연상시켜서 붙은 이름이다.

![[Pasted image 20240202024338.png]]

## 예시

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
