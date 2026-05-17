- [[노드(Node.js)]]는 현재 실행중인 노드 프로세스에 대한 정보를 가지고 있다.


## process [[객체(Object)]]의 종류

```js
console.log(process.cwd()); // 현제 프로세스가 실행되는 위치 

console.log(process.version);// node.js의 버전

console.log(process.arch); // 프로세서의 아키텍처(arm/ia32/x64) 

console.log(process.platform); // 플랫폼(win32/linux/sunos/freebsd/darwin) 맥은 darwin

console.log(process.pid); // 현재 프로세스의 아이디

console.log(process.uptime()); // 프로세스가 시작된 후 흐른 시간

console.log(process.execPath); // 노드의 경로

console.log(process.cwd()); // 현제 프로세스가 실행되는 위치

console.log(cpuUsage()); // 현재 cpu 사용률
```

## env 파일을 사용하는 방법

```js
const secrtId = process.env.SECRET_ID; 
const secrtCode = process.env.SECRET_CODE; 
```

## 노드가 사용할 수 있는 메모리를 지정하는 옵션

```js
NODE_OPTIONS==--max-old-space-size=8192
UV_THREADPOOL_SIZE=8 
```


## process.nextTick()

- 그 어떤 [[비동기(asynchronous)]] [[메서드(Method)]]보다 빨리 실행되게 만드는 [[메서드(Method)]]이다.

```js
setImmediate(() => {
	console.log('immediate);
});

process.nextTick(() => { // 가장 먼저 실행
	console.log('nextTick');
});

setTimeout(() => {
	console.log('timeout');
}, 0);

Promise.resolve().then(() =>  console.log('promise'));

// >> nextTick
// >> promise
// >> timeout // setImmediate와 setTimeout는 랜덤성이 있음
// >> immediate
```