- Promise.any는 가장 먼저 resolve가 된 [[Promise]]의 값 하나를 데이터로 넘겨준다.
- 따라서 Promise.any는  Promise [[배열(Array)]]이 모두 실패해야지만 실패한다.
### 예시

```js
function getName() {
	return new Promise((resolve, reject) => {
		setTimeout(() => {
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
```

```js
const promise = Promise.any([getName(), getTodo()]);
promise
	.then((data) => {
		console.log(data);
	})
	.catch((error) => {
		console.log(error);
	}); // 만약 모든 Promise가 실패하면 AggregateError: All promises were rejected
```
