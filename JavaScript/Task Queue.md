## Event Loop & Task Queue

- 아래 코드는 [[이벤트 루프(Event Loop)]]와 테스크큐의 동작과정을 파악하기 위핸 예시이다.

```js
console.log("start", new Date())

setTimeout(()=>{
	console.log("setTimeout", new Date())
}, 0);

const wakeUpTime = Date.now() + 1000;

while (Date.now() < wakeUpTime) {}

console.log("test1");
console.log("test2");
console.log("test3");
console.log("end", new Date())
```

- 단순 console.log를 실행한다.
- setTimeout이 동작한다. 동작시간은 0초로 설정하였다.
- 이후 현재시간의 1초뒤의 시간을 wakeUpTime에 작성하여 while문에서 시간으로 체크한다.
- 1초동안 강제로 동기화를 시켜서 페이지가 살짝 멈추게 된다.
- 이후 단순한 console.log들을 출력한다.

- 아래 출력 결과이다.

![](https://blog.kakaocdn.net/dn/ckcVof/btsiFNQKLzV/oINA5XSCsQKSrKsrSBHSDK/img.png)

-  [[setTimeout()]] 메서드가 2번째에 있고 동작시간을 0으로 처리했음에도 콜백안에 있는 console.log는 가장 나중에 출력되고 있는걸 볼 수 있다.  

- 심지어 4번째 while문에서 강제로 블로킹 처리를 해서 페이지가 멈추는 현상까지 발생는데 어떻게 이런 결과가 나왔을까?  

- 이런 현상에 대해서는 [[이벤트 루프(Event Loop)]], 테스트 큐에 대해 알아야한다.

- 먼저 테스크 큐는 [[콜백 함수(callback)]]들이 대기하는 큐(Queue)형태이다.  
- Queue는 선입선출 구조로 먼저 들어온 것들이 먼저 나간다.  

- 위 예제의 실제 동작을 설명하자면 아래와 같다.
	- [[Call Stack]]에 console,log("start")가 push되고 출력되면서 pop된다.
	- setTimeout이 콜스택에 push되고 콜백 내용이 테스크 큐에 등록되면서 pop된다.
	- 0ms가 지나고 이벤트 큐는 콜 스택이 비워질때까지 기다리면서 콜백을 실행시킬 준비를 한다.
	- wakeUpTime의 변수가 메모리 힙에 올라가고 이후부턴 콜스택들이 동작된다...

위와 같은 비동기 처리나 I/O의 작업이 자바스크립트에서 발생하면 콜백함수가 콜스택에서 처리되는게 아니라 별도의 테스크 큐라는 곳에 쌓였다가 이벤트 루프에 의해 실행되기 때문에 뜻밖의 결과를 얻을 수 있다.

## ⭐브라우저의 내부 환경

자바스크립트의 엔진을 구동하는 환경의 브라우저의 내부 모습은 아래와 같다.

![](https://blog.kakaocdn.net/dn/odxEo/btsiM9eNaUQ/DhCi8WlybuE3GSrhr7TukK/img.png)

그림에서도 볼 수 있듯이 개발하면서 자주 사용했던 `Ajax`, `setTimeout`과 같은 비동기 처리들은 자바스크립트가 해주는게 아니라 Web API영역에 따로 정의되어 있다. Node.JS에서는 동시성을 위해 `libuv`라이브러리를 도입했는데, 이 `libuv`라이브러리가 `이벤트 루프`를 제공한다.

정리하자면 비동기 작업을 하게되면 Node.js의 API가 호출되고, 그러면서 작성된 콜백 함수들은 `이벤트 루프`에 의해 관리가 되고 실행이 된다.