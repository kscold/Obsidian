- [[함수(Function)]]가 호출이 되면 쌓이는 곳이다. 
- 대신 함수가 쌓이는 순서와는 반대로 실행된다.

- 자바스크립트에서 함수를 호출하면 Call Stack이라는 곳에 호출 순서대로 차곡차곡 쌓인다.
- 그러고 나서, Stack은 맨 마지막에 호출된 함수가 맨 먼저 반환한다.

- Last in Frist Out, 먼저 들어온 것이 먼저 나간다라는 의미로, LIFO 구조이다.

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