- [[노드(Node.js)]]에서 스레드 풀(thread pool)은 I/O 작업을 처리하는 데 사용되는 스레드의 집합이다.

- [[노드(Node.js)]]는 기본적으로 싱글 스레드로 동작하므로, 블로킹되는 I/O 작업(예: 파일 시스템 액세스, 네트워크 호출 등)을 처리할 때는 이러한 작업을 [[비동기(asynchronous)]]적으로 수행하는 것이 중요하다.

- 이를 위해 노드는 내부적으로 스레드 풀을 사용하여 I/O 작업을 [[비동기(asynchronous)]]적으로 처리한다.

- 노드의 스레드 풀은 기본적으로 4개의 스레드를 사용한다. 

- 이는 libuv 라이브러리에서 관리됩니다. 

- 각 스레드는 I/O 작업을 처리하고, 작업이 완료되면 [[콜백 함수(Callback Function)]]를 호출하여 결과를 반환한다. 

- 이렇게 함으로써, 노드는 I/O 작업을 비동기적으로 처리하면서도 블로킹되지 않고, 다른 작업을 처리할 수 있다.
- 스레드 풀의 크기는 노드의 환경 변수를 통해 조정할 수 있다.

- 이를 통해 노드 애플리케이션의 성능을 최적화할 수 있다. 
- 그러나 스레드 풀의 크기를 너무 크게 설정하면, [[실행 컨텍스트(execution context)]] 스위칭 오버헤드가 증가하여 성능이 저하될 수 있으므로 적절한 크기로 설정하는 것이 중요하다.

## 암호화하는 cryto [[모듈(Module)]]을 통한 스레드 풀 설정

- UV_THREADPOOL_SIZE=갯수 로 설정하는 스레드 풀은 일회성이므로 실행할 때마다UV_THREADPOOL_SIZE=8 node threadpool의 코드로 스레드풀의 최대를 지정하거나 코드 안에서 [[export]]문을 수정해야한다.

```js
const crypto = require('crypto');

const pass = 'pass';
const salt = 'salt';
const start = Date.now();

  
// 아래의 코드를 통해 노드js는 기본적으로 4개씩 코어를 사용하여 그룹을 지어 처리한다는 것을 알 수 있음
// 따라서 UV_THREADPOOL_SIZE=8 node threadpool의 코드로 스레드풀의 최대를 지정할 수 있음
crypto.pbkdf2(pass, salt, 1_000_000, 128, 'sha512', () => {
	console.log('1', Date.now() - start);
});

crypto.pbkdf2(pass, salt, 1_000_000, 128, 'sha512', () => {
	console.log('2', Date.now() - start);
});

crypto.pbkdf2(pass, salt, 1_000_000, 128, 'sha512', () => {
	console.log('3', Date.now() - start);
});

crypto.pbkdf2(pass, salt, 1_000_000, 128, 'sha512', () => {
	console.log('4', Date.now() - start);
});

crypto.pbkdf2(pass, salt, 1_000_000, 128, 'sha512', () => {
	console.log('5', Date.now() - start);
});

crypto.pbkdf2(pass, salt, 1_000_000, 128, 'sha512', () => {
	console.log('6', Date.now() - start);
});

crypto.pbkdf2(pass, salt, 1_000_000, 128, 'sha512', () => {
	console.log('7', Date.now() - start);
});

crypto.pbkdf2(pass, salt, 1_000_000, 128, 'sha512', () => {
	console.log('8', Date.now() - start);
});
```