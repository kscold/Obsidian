- child_process는 [[노드(Node.js)]]에서 child_process모듈을 이용하여 현재 쉘이 아닌 다른 쉘을 이용하는 방법으로 node의 특징인 싱글스레드의 성능문제의 해결 방법중 하나이다.

- [[노드(Node.js)]]에서 child_process를 호출하고 실행을 할때 child_process를 호출하고 실행하는 프로세스를 부모 프로세스라고 하고 실행되는 프로세스를 자식 프로세스라고 한다.


## exec를 사용하여 간단한 명령을 실행시키는 예시


```js
const exec = require('child_process').exec;

let process = exec('ls'); // 표준 입력을 실행


process.stdout.on('data', function (data) { // 성공했을 시에 표준 출력을 보여줌
	console.log(data.toString());
});


process.stderr.on('data', function (data) { // 실패했을 시에 표준 에러를 보여줌
	console.error(data.toString());
});
```

##  spawn을 사용하여 다른 언어를 실행시키는 예시


```js
const spawn = require('child_process').spawn;
  
const process = spawn('python3', ['test.py']); // 외부의 다른 파이썬 파일을 실행

process.stdout.on('data', function (data) {
	console.log(data.toString()); // 정상적으로 실행된다면 표준출력
});

process.stderr.on('data', function (data) {
	console.error(data.toString()); // 정상적으로 실행되지 않는다면 표준에러
});
```