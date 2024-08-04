- [[에러(error)]]가 발생하면 스크립트는 ‘죽고’(즉시 중단되고), 콘솔에 에러가 출력된다.

- 그러나 try catch 문법을 사용하면 스크립트가 죽는 걸 방지하고, 에러를 "잡아서(catch)" 더 합당한 무언가를 할 수 있게 된다.

## try catch 문법

- ‘try…catch’ 문법은 'try’와 'catch’라는 두 개의 주요 블록으로 구성됩니다.

```js
try {
	// 코드...
} catch(err) {
	// 에러 핸들링
}
```

- try…catch 동작 알고리즘은 다음과 같다.

1. 먼저, `try {...}` 안의 코드가 실행된다.
2. 에러가 없다면, `try` 안의 마지막 줄까지 실행되고, `catch` 블록은 건너뛴다.
3. 에러가 있다면, `try` 안 코드의 실행이 중단되고, `catch(err)` 블록으로 제어 흐름이 넘어간다. ([[변수(Variable)]] `err`(아무 이름이나 사용 가능)는 무슨 일이 일어났는지에 대한 설명이 담긴 에러 [[객체(Object)]]를 포함함)

- 이렇게 `try {…}` 블록 안에서 에러가 발생해도 `catch`에서 에러를 처리하기 때문에 스크립트는 죽지 않는다.

## 예시

- 아래 코드는 에러가 없는 예시이다.

```js
try {
	alert('try 블록 시작'); // (1) 
	// ...에러가 없음
	alert('try 블록 끝'); // (2)
} catch(err) {
	alert('에러가 없으므로, catch는 무시됩니다.'); // (3)
} 
```

- 에러가 있는 예시이다.

```js
try {
	alert('try 블록 시작'); // (1)
	lalala; // 에러, 변수가 정의되지 않음
	alert('try 블록 끝(절대 도달하지 않음)'); // (2)
} catch(err){
	alert('에러가 발생했습니다!'); // (3)
}
```

- try catch는 오직 [[런타임(runtime)]] 에러에만 동작한다.

- try catch는 실행 가능한(runnable) 코드에만 동작한다.
- 실행 가능한 코드는 유효한 자바스크립트 코드를 의미합니다.
- 중괄호 짝이 안 맞는 것처럼 코드가 문법적으로 잘못된 경우엔 try catch가 동작하지 않는다.

- 자바스크립트 엔진은 코드를 읽고 난 후 코드를 실행하다.
- 코드를 읽는 중에 발생하는 에러는 'parse-time 에러’라고 부르는데, 엔진은 이 코드를 이해할 수 없기 때문에 parse-time 에러는 코드 안에서 복구가 불가능하다.

- try catch는 유효한 코드에서 발생하는 에러만 처리할 수 있다.
- 이런 에러를 ‘런타임 에러(runtime error)’ 혹은 '예외(exception)'라고 부른다.

- try catch는 동기적으로 동작한다.
- [[setTimeout()]]처럼 ‘스케줄 된(scheduled)’ 코드에서 발생한 예외는 try catch에서 잡아낼 수 없다.

```js
try {
	setTimeout(function() {
		noSuchVariable; // 스크립트는 여기서 죽음
	}, 1000);
} catch (e) {
	alert( "작동 멈춤" );
}
```

- [[setTimeout()]]에 넘겨진 [[익명 함수]]는 엔진이 try catch를 떠난 다음에서야 실행되기 때문이다.
- 스케줄 된 함수 내부의 예외를 잡으려면, try catch를 반드시 함수 내부에 구현해야 한다.

```js
setTimeout(function() {
	try {
		noSuchVariable; // 이제 try..catch에서 에러를 핸들링 할 수 있음
	} catch {
		alert("에러를 잡았습니다!");
	}
}, 1000);
```
