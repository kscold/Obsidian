- Promise.race는 Promsie [[배열(Array)]] 중에서 가장 먼저 완료되는 Promise 값 하나를 반환한다.
- 가장 빨리 데이터가 오는 Promise가 Promise.race조건인데 이때, 실패한 경우도 포함이다.

## 예시

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
const promise = Promise.race([getName(), getTodo()]);
promise
	.then((data) => {
		console.log(data); // 둘 다 성공했다면 시간이 짧은 철수 출력
	})
	.catch((error) => {
		console.log(error);
	}); // 만약 모든 Promise가 실패하면 AggregateError: All promises were rejected
```
