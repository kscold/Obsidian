- [[노드(Node.js)]]는 기본적으로 싱글 스레드이다.
- [[노드(Node.js)]]는 어플리케이션은 하나의 코어에서 실행되기 때문에 CPU가 멀티 코어인 경우에는 하나를 제외한 나머지 코어를 활용하지 못하는 꼴이 되는데, 이는 컴퓨터가 가진 성능을 온전히 발휘하지 못하는 일이 되기 때문에, [[노드(Node.js)]]에서는 클러스터라는 기능을 제공하고 있다.

- 클러스터는 [[노드(Node.js)]]에서 제공되는 코어 [[모듈(Module)]]의 일부로, CPU의 코어 수보다 한 개 적은 수이다.

- 하나는 마스터 프로세스가 되어야 하기 때문의 워커 프로세스를 생성하여, 각기 다른 프로세스에서 코드가 실행되도록 도와준다.

```js
// cluster Module을 사용하여 cpu 숫자만큼의 process를 생성함
const cluster = require('cluster');
const http = require('http');
const numCPUs = require('os').cpus().length; // cpu 갯수만큼 알아낼 수 있음

if (cluster.isMaster) {
	// 마스터 스레드인지 확인
	console.log(`마스터 프로세스 아이디: ${process.pid}`);
	for (let i = 0; i < numCPUs; i += 1) {
		cluster.fork(); // 쓰레드를 fork를 함
	}
	
	cluster.on('exit', (worker, code, signal) => {
		console.log(`${worker.process.pid}번 워커가 종료되었습니다.`);
		console.log('code', code, 'signal', signal);
		cluster.fork(); 
	});
	
} else {
	http.createServer((req, res) => {
		res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' });
		res.write('<h1>Hello Node!</h1>');
		res.end('<p>Hello Cluster!</p>');
		
		setTimeout(() => {
			process.exit(1);
		}, 1000);
	}).listen(8080);
	
	console.log(`${process.pid}번 워커 실행`);
}
```

- 위 코드는 [[os]] [[모듈(Module)]]로부터 CPU의 코어 수를 가져온다. 

- isPrimary와 isMaster [[노드(Node.js)]] 16 version에 등장하므로 LTS에서는 확인할 수 없다.
- 현재 코드가 동작할 때의 코드가 master process인지를 알려주는 프로퍼티이다.

```
72079번 워커 실행
72078번 워커 실행
72073번 워커 실행
72077번 워커 실행
72075번 워커 실행
72074번 워커 실행
72080번 워커 실행
72076번 워커 실행
72082번 워커 실행
72081번 워커 실행
```

- master process가 실행된 다음, Worker들이 실행된다.
- 또한 각 프로세스 아이디는 실행될 때마다 바뀌고 총 9개의 프로세스가 실행되고 있다.

- 사실 CPU의 코어 수만큼 프로세스를 여는 게 맞지만, 실질적인 동작은 Worker들이 처리하기 때문에 마스터 프로세스를 포함해, 코어보다 1개 많은 프로세스가 열렸다.