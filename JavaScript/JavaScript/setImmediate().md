- setImmediate()와 [[setTimeout()]]은 비슷하지만, 언제 호출되느냐에 따라 다르게 행동한다.

- 타이머가 실행되는 순서는 호출되는 콘텍스트 시점에 따라 다르다.

- 만약 둘 다 **메인 모듈에서 호출**되면, 프로세스의 성능에 따라 호출되는 시점이 다르다. 
- 이것은 머신의 다른 애플리케이션에 의해 영향을 받을 수 있다.

- 따라서 [[노드(Node.js)]] 환경에서는 [[setTimeout()]] 대신 setImmediate()만 사용하는 것이 좋다.


## setImmediate()를 사용해야하는 경우

- 예를 들어, 아래 코드처럼 I/O 사이클에 있지 않은 스크립트를 실행시킬 때(메인 모듈에 있는 경우), 두 개의 타이머가 실행되는 순서는 확실하지 않고, 프로세스의 퍼포먼스에 의해 달라진다.

```js
// Execute
setTimeout(() => {
	console.log('timeout'); 
}, 0);

setImmediate(() => {
	console.log('immediate');
});


// Output 1
timeout
immediate

// Output 2
immediate
timeout
```

[![이벤트루프](https://blog.kakaocdn.net/dn/bCotLv/btriBBKbbjl/bwq3ysvgirFBJdNiohbes0/img.png)](https://blog.kakaocdn.net/dn/bCotLv/btriBBKbbjl/bwq3ysvgirFBJdNiohbes0/img.png)

1. setTimeout()의 Callback이 timers에 등록된다.
2. setImmediate()의 Callback이 check에 등록된다.
3. A. timers phase에 도달했는데, 1ms 전에 timer phase를 통과 한 경우 check phase의 setImmediate()가 먼저 실행, B. timers phase에 도달했는데, 1ms가 이미 되었다면 setTimeout() 이 먼저 실행한다.

- setTimeout(func, 0)의 0ms는 결국 1ms로 동작한다.

- 그런데 두 개의 호출을 I/O 사이클에 넣으면, immediate 콜백이 항상 먼저 실행된다.
- setTimeout( ) 보다 setImmediate( )를 사용하는 것의 장점은 I/O 사이클에 스케줄 됐다면 얼마나 많은 타이머가 있는 것에 상관없이 [[setTimeout()]]이 언제나 먼저 실행된다는 것이다.

```js
// Execute
const fs = require('fs');

fs.readFile('a.js', (result) => {
	setTimeout(() => {
		console.log('timeout');
	}, 0);
	
	setImmediate(() => {
		console.log('immediate');
	});
});


// Output
immediate
timeout
```

[![이벤트루프](https://blog.kakaocdn.net/dn/bYB6xY/btriB5xrhUt/obhtbIfiHhfYM8L8gJkkW0/img.png)](https://blog.kakaocdn.net/dn/bYB6xY/btriB5xrhUt/obhtbIfiHhfYM8L8gJkkW0/img.png)

[![이벤트루프](https://blog.kakaocdn.net/dn/m6A47/btriB64c8Pp/0ZDb8thT7ArZfvLT4LItMK/img.png)](https://blog.kakaocdn.net/dn/m6A47/btriB64c8Pp/0ZDb8thT7ArZfvLT4LItMK/img.png)

1. fs.readFile() 이 실행되고, Callback이 poll에 들어갑니다.
2. poll phase에 진입하고, Callback이 실행되므로 setTimeout() 의 Callback이 timers phase에 들어갑니다.
3. setImmediate() 의 Callback이 check phase에 들어갑니다.
4. poll phase를 모두 소진했으니, 다음 phase인 check phase로 이동할 것입니다.

- 같은 I/O 주기 내에서는 Immediate가 먼저 실행됩니다.