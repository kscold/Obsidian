- 일정한 크기로 나눠서 여러 번에 걸처서 처리하는 것을 의미한다.

- [[버퍼(Buffer)]](또는 청크)의 크기를 작게 만들어서 주기적으로 데이터를 전달한다.

- [[노드(Node.js)]] 스트림의 경우 [[비동기(asynchronous)]]이고 순서대로 전달된다.


## 스트리밍

- 일정한 크기의 데이터를 지속적으로 전달하는 작업을 의미한다.

![[Pasted image 20240330180401.png]]

- 아래의 예시는 최대 16byte로 쪼개어 파일을 쪼개서 스트리밍하는 방식이다.
- 따라서 대용량 파일 서버를 이용할 때 필수적이다.

```js
const fs = require('fs');
const readStream = fs.createReadStream('./readme3.txt', { highWaterMark: 16 });
// 스트림 별로 조각 내서 읽음, 그러나 기본적으로 64kb를 한번에 읽기 때문에 16byte로 쪼개개 설정함

  
// 따라서 대용량 파일 서버를 이용할 때 필수적
const data = [];

readStream.on('data', (chunk) => {
	data.push(chunk);
	console.log('data:', chunk, chunk.length);
});

readStream.on('end', () => {
	console.log('end:', Buffer.concat(data).toString());
});

readStream.on('error', (err) => {
	console.log('error:', err);
});
```