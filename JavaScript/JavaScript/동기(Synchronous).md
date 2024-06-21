- 자바스크립트는 한 번에 하나의 작업을 수행한다.
- 동시적으로 값들을 처리하는 것이 아니라 스택에 의해 하나하나 처리되는 과정을 동기 프로그래밍이라고 한다.
- 따라서 이러한 동작을 단일 스레드(싱글 스레드), 동기(Synchronous)라고 부른다.

- 특히 rest api의 동기적으로 값을 받고 그 이후에 프론트적인 렌더링을 주로 많이 일으키키 때문에 [[cors]] 오류와 동기적인 작업을 위해 동기적으로 바꾸는 방법을 알고 있어야한다.

- [[비동기(asynchronous)]]와 반대되는 개념이다.

## 왜 동기적일까?

- 자바스크립트의 Engine(V8)을 확인해보면 알 수 있다.

![](https://blog.kakaocdn.net/dn/8XRnQ/btqFQKwWMDa/pX0jhdyajUYb1OOQKMfR01/img.png)

- 위의 이미지 Memory [[힙(Heap)]]과 [[호출 스택(Call Stack)]]은 자바스크립트의 엔진의 주요 구성 요소이다.

```js
console.log("hi"); 
console.log("hello"); 
console.log("bye");

// >> hi
// >> hello
// >> bye
```

- 위 코드의 순서대로 처음에 console.log("hi")가 호출되면서 Call Stack에 먼저 쌓이고, hi를 반환하면 console.log("hi")는 Call Stack에서 사라진다.
- 하지만, hi를 반환하기 전까지 다음 작업은 수행할 수 없다.

- 이 순서대로 console.log("hi")가 사라지고 hi를 반환한다.
- 그러고 나서, console.log("hello")가 쌓이고 hello를 반환하면, console.log("hello")가 사라진다.
- 또, 그러고 나서 console.log("bye")가 쌓이고 bye를 반환하면 console.log("bye")가 마지막으로 사라진다.
- 이러한 방식으로 진행되기 때문에 콘솔 창에 순서대로 "hi", "hello", "bye"가 찍히는 것이다.
