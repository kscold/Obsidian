- Promise.allSettled를 사용하면 [[Promise]]가 성공했는지 실패했는지 세세하게 알려준다.

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
const promise = Promise.allSettled([getName(), getTodo()]);
promise.then((data) => {
	console.log(data);
})
```

```bash
// 실행결과
(2) [{…}, {…}]
	0:
		reason: Error: 에러: 이름이 없어요 // 만약 실패했다고 가정
		status: "rejected"
		[[Prototype]]: Object
	1:
		status: "fulfilled"
		value: "밥먹기"
		[[Prototype]]: Object
	length: 2
	[[Prototype]]: Array(0)
```
