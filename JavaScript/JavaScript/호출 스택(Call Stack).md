- [[함수(Function)]]가 호출이 되면 쌓이는 곳이다. 
- 대신 [[함수(Function)]]가 쌓이는 순서와는 반대로 실행된다.

- 자바스크립트에서 함수를 호출하면 Call Stack이라는 곳에 호출 순서대로 차곡차곡 쌓인다.
- 그러고 나서, Stack은 맨 마지막에 호출된 함수가 맨 먼저 반환한다.

- Last in Frist Out, 먼저 들어온 것이 먼저 나간다라는 의미로, LIFO 구조이다.

## 호출 스택의 과정

![[Pasted image 20240310015130.png]]

- Anonymous는 가상의 전역 컨텍스트이다.(항상 있다고 생각하는게 편하다.)
- 함수 호출 순서대로 쌓이고, 역순으로 실행된다.
- 함수 실행이 완료되면 스택에서 빠진다.
- LIFO 구조라서 스택이라고 불린다.

```js
function first() {
	second();
	console.log('첫 번째');
}

function second() {
	third();
	console.log('두 번째');
}


function third() {
	console.log('세 번째');
}

first()

// >> 세 번째
// >> 두 번째
// >> 첫 번째
```

- 밑의 코드는 [[비동기(asynchronous)]] 코드를 실행할 때 예시이다.

```js
function run() {
	console.log('3초 후 실행');
}

console.log('시작');
setTimeout(run, 3000);
console.log('끝');

// >> 시작
// >> 끝
// >> 3초 후 실행
```

- 위 코드의 순서을 예측해보면 시작, 끝, 3초 후 실행으로 이루어진다.
- 따라서 호출 스택만으로는 설명할 수 없기 때문에 [[이벤트 루프(Event Loop)]]의 개념이 들어간다.
## 예시

```js
console.log("hi"); 
console.log("hello"); 
console.log("bye");

// >> hi
// >> hello
// >> bye
```

![[Pasted image 20240131013317.png]]

![[Pasted image 20240131013502.png]]

![[Pasted image 20240131013509.png]]


