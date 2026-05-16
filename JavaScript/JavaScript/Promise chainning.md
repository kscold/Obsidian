- [[Promise]]로 반환되는 데이터들을 [[then()]], [[catch()]], [[finally()]] [[메서드(Method)]]로 꼬리를 물며 [[비동기(asynchronous)]] 작업을 **순차적으로 연결**하는 패턴이다.
- 실제 실행은 여전히 비동기지만, 콜백 지옥처럼 중첩되지 않고 **위에서 아래로 읽히는 코드**가 되어 마치 동기처럼 보이게 만든다.
- 마치 고리가 연결되는 것을 연상시켜서 붙은 이름이다.

- 각 [[then()]]은 새로운 [[Promise]]를 반환하므로 다음 then()이 그 결과를 받아 이어진다.
- 중간 then()에서 [[Promise]]를 return하면 그 Promise가 resolve될 때까지 다음 then()이 기다린다.

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
