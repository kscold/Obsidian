- [[노드(Node.js)]] 서버에서 가끔 주소창에 한글을 입력하면 서버가 처리하지 못해는 경우가 발생하기 때문에 encodeURIComponet [[객체(Object)]]를 사용한다.

- 따라서 아스키코드는 한글이 지원되지 않기 때문에 주소에는 영어만 사용하는게 좋다.

 - 반대로 다시 서버에서 한글을 해독할 때는 decodeURIComponent를 사용하면 된다.

## 예시

```js
(async () => {
	try {
		const result = await axios.get(`https://www.zerocho.com/api/search/${encodeURIComponent('노드')}`);
		console.log(result);
		console.log(result.data); // {}
	} catch (error) {
		console.error(error)
	}
})();
```