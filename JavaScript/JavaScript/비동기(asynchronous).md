- 동시적으로 실행되는 작업을 비동기 작업이라고 한다.
- 자바스크립트는 단일 스레드, [[동기(Synchronous)]]식으로 동작한다.
- 하지만 비동기는 어떠한 요청을 보내면 그 요청이 끝날 때까지 기다리는 것이 아니라, 응답에 관계없이 바로 다음 동작이 실행되는 방식을 말한다.

- 이때 비동기 작업을 마치 [[동기(Synchronous)]] 작업의 결과와 같이 데이터을 받기 위해서는 [[콜백 함수(Callback Function)]]를 이용한 콜백 지옥이나 [[async await]]와 같은 문법을 사용한다.

- 또한 비동기 작업은 실패할 가능성도 있으므로 이에 대해 대비책인 try/catch 구문이나 [[Promise]] [[메서드(Method)]]를 사용하여 처리해야한다.

## 자바스크립트가 멀티스레드(비동기)처럼 동작할 수 있는 이유

- 자바스크립트가 어떻게 화면 전환하면서 HTTP 요청이나 여러 [[이벤트(event)]]를 동시에 동작시킬 수 있는 이유는 실행 환경([[런타임(runtime)]])과 관련이 있다.
- 브라우저에서는 자바스크립트 엔진만으로 동작하지 않는다.

![](https://blog.kakaocdn.net/dn/bMlLfs/btqFQ9i1iD3/ZQE2tqi7lx7LUhTwK1tDtK/img.png)

- 브라우저에서의 자바스크립트 실행 환경(Runtime)에서는 자바스크립트 엔진 자체가 제공하지 않는 일부 기능인 [[DOM(Document Object Model)]] 조작이나 [[Ajax(Asynchronous JavaScript and XML)]] 같은 **비동기 처리를 위한** [[Web API]]를 제공**한다.

- 또, 이를 제어하기 위한 과정을 [[이벤트 루프(Event Loop)]]라고 부르모 이 [[이벤트 루프(Event Loop)]] 안에는 [[호출 스택(Call Stack)]]과  [[테스크 큐(Task Queue)]]가 존재한다.

### 비동기(asynchronous) 동작 원리

1. [[호출 스택(Call Stack)]]에서 비동기 함수가 호출되면 [[호출 스택(Call Stack)]]에 먼저 쌓였다가 [[Web API]](백그라운드)로 이동한 후 해당 [[함수(Function)]]가 등록되고 [[호출 스택(Call Stack)]]에서 사라진다.

2. 백그라운드([[Web API]]에서 비동기 함수의 [[이벤트(event)]]가 발생하면, 해당 [[콜백 함수(Callback Function)]]는 [[테스크 큐(Task Queue)]]에 이동(push)) 된다.

3. 이제 [[호출 스택(Call Stack)]]이 비어있는지 [[이벤트 루프(Event Loop)]]가 확인을 하는데 만약 비어있으면, [[호출 스택(Call Stack)]]에 [[테스크 큐(Task Queue)]]에 있는 [[콜백 함수(Callback Function)]]를 이동(push)시켜 준다.

4. 다시 [[호출 스택(Call Stack)]]에 들어온 [[함수(Function)]]는 실행이 되고 실행이 끝나면 [[호출 스택(Call Stack)]]에서 사라진다.


## 비동기 작업을 콜백 함수를 중첩하여 동기 형식으로 처리(콜백 지옥)

- [[setTimeout()]]이 비동기적으로 동작하는 자바스크립트의 내장 [[함수(Function)]]이기 때문에 이를 [[동기(Synchronous)]]적으로 호출하기 위해서 [[콜백 함수(Callback Function)]]를 중첩하여 표현할 수 있다.

```js
// 1. 로그인
function login(username, callback) {
	setTimeout(() => {
		callback(username);
	}, 1000)
}

// 2. 장바구니에 넣기
function addToCart(product, callback) {
	setTimeout(() => {
		callback(product);
	}, 1000)
}

// 3. 결제하기
function makePayment(cardNumber, product, callback) {
	setTimeout(() => {
		callback(cardNumber, product);
	}, 1000);
}

login('별코딩', (username) => {
	console.log(`${username}님 안녕하세요`);
	addToCart("감자", (product) => {
		console.log(`${product}를 장바구니에 넣었습니다.`);
		makePayment("0000000000000000", product, (cardNumber, product) => {
			console.log(`${cardNumber.slice(0, 6)}로 ${product}를 구매했습니다.`);
		});
	});
});
```

## Promise로 동기처리

- [[콜백 함수(Callback Function)]] 대신 [[Promise]] 동작으로 바꾸는 예시이다.

```js
// 1. 로그인
function login(usernaem) {
	return new Promise((reslove, reject) => {
		setTimeout(() => {
			if (username) {
				resolve(username);
			} else {
				reject(new Error('아이디를 입력해주세요'))
			}
		}, 1000);
	});
}

// 2. 장바구니에 넣기
function addToCart(product) {
	return new Promise((resolve, reject) => {
		setTimeout(() => {
			if (product) {
				resolve(product);
			} else {
				reject(new Error('장바구니에 넣을 상품이 없어요!'));
			}
		}, 1000);
	});
}

// 3. 결제하기
function makePayment(cardNumber, product) {
	return new Promise((resolve, reject) => {
		seTimeout(() => {
			if (cardNumber.length !== 16) {
				reject(new Error("잘못된 카드 번호 입니다."))
				return
			}
			
			if (!product) {
				reject(new Error("결제할 상품을 넣어주세요."))
				return
			}
			resolve(product);
		}, 1000);
	});
}

login('')
	.catch((error) => {
		return '익명'; // 만약 에러가 발생한다명 바로 resolve가 되어 return 됨
	})
	.then((username) => {
		console.log(`${username}님 환영합니다.`);
		return addToCart("감자");
	})
	.catch((error) => {
		return '옥수수';
	})
	.then((product) => {
		console.log(`${product}를 장바구니에 넣었어요`);
		return makePayment('1234123412341234', product)
	})
	.then((product) => {
		console.log(`${product}를 결제를 완료했습니다.`);
	})
	.catch((error) => {
		console.log(error.message);
	}).finallt(() => {
		console.log('마무리 작업');
	})

// return 생략
login('')
	.catch(() => '익명')
	.﻿﻿then((username) => addToCart('')
	.﻿catch(() => '옥수수')
	.then((product) => makePayment('1234123412341234', product))
	.then(product) => (console.log(`${product}를 결제를 완료했습니다.`))
	.catch((error) => console.log(error.message))
	.finally(() => console. log('마무리 작업'));
```