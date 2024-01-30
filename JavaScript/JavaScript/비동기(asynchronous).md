- 동시적으로 실행되는 작업을 비동기 작업이라고 한다.
- 자바스크립트는 단일 스레드, 동기식으로 동작한다.
- 하지만 비동기는 어떠한 요청을 보내면 그 요청이 끝날 때까지 기다리는 것이 아니라, 응답에 관계없이 바로 다음 동작이 실행되는 방식을 말한다.

- 이때 비동기 작업을 [[동기(Synchronous)]] 마치 동기작업의 결과와 같이 데이터을 받기 위해서는 [[콜백 함수(Callback Function)]]를 이용한 콜백지옥이나 [[async await]]와 같은 문법을 사용한다.

## 자바스크립트가 멀티스레드 처럼 동작할 수 있는 이유

- 자바스크립트가 어떻게 화면 전환하면서 HTTP 요청이나 여러 [[이벤트(event)]]를 동시에 동작시킬 수 있는 이유는 실행 환경(Runtime)과 관련이 있다.
- 브라우저에서는 자바스크립트 엔진 만으로 동작하지 않는다.

![](https://blog.kakaocdn.net/dn/bMlLfs/btqFQ9i1iD3/ZQE2tqi7lx7LUhTwK1tDtK/img.png)

- 브라우저에서의 자바스크립트 실행 환경(Runtime)에서는 자바스크립트 엔진 자체가 제공하지 않는 일부 기능인 [[DOM(Document Object Model)]] 조작이나 [[Ajax의(Asynchronous JavaScript and XML)]] 같은 **비동기 처리를 위한** [[Web API]]를 제공**한다.

- 또, 이를 제어하기 위해 이벤트 루프(Event Loop), 이벤트 큐(Callback Queue 혹은 task Queue)가 존재한다.

## 비동기(asynchronous) 동작 원리

1. [[콜 스택(Call Stack)]]에서 비동기 함수가 호출되면 Call Stack에 먼저 쌓였다가 [[Web API]](혹은 백그라운드라고도 한다)로 이동한 후 해당 함수가 등록되고 Call Stack에서 사라진다.
    
2. Web API(백그라운드)에서 비동기 함수의 [[이벤트(event)]]가 발생하면, 해당 [[콜백 함수(Callback Function)]]는 Callback Queue에 push(이동) 된다.
    
3. 이제 [[콜 스택(Call Stack)]]이 비어있는지 [[이벤트 루프(Event Loop)]]가 확인을 하는데 만약 비어있으면, Call Stack에 [[테스크 큐(Task Queue)]]Callback Queue에 있는 콜백 함수를 넘겨준다.(push)
    
4. Call Stack에 들어온 함수는 실행이 되고 실행이 끝나면 Call Stack에서 사라진다.