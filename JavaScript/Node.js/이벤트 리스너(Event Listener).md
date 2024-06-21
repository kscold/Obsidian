- [[노드(Node.js)]]에서의 [[이벤트(event)]]를 단순하게 말하자면 한쪽에서 다른 쪽으로 알림 메시지를 보내는 것과 비슷하다.

- [[콜백 함수(Callback Function)]]는 [[비동기(asynchronous)]] [[함수(Function)]]에서 결과 값을 반환할 때 호출되는 반면에, 이벤트는 옵저버 패턴에 의해 작동 된다.
- 이벤트 리스너(Event Listener)가 [[이벤트(event)]]가 발생하기를 기다리다가, [[이벤트(event)]]가 발생하면 이벤트 핸들링 [[함수(Function)]]를 실행한다.

- [[노드(Node.js)]]에는 이런 [[이벤트(event)]]를 보내고 받을 수 있도록 EventEmitter [[클래스(Class)]]가 정의되어 있다. 
- 객체들은 EventEmitter를 [[상속(Inheritance)]]받아, on()과 emit() [[메서드(Method)]] 등을 사용하여 [[이벤트(event)]]를 주고 받는다.





## [[노드(Node.js)]]의 [[이벤트(event)]] 핸들링  [[메서드(Method)]]
### [[on()]] 메개변수, (eventName, listener)

- 지정한 이벤트의 리스너를 추가한다.
### once(eventName, listener)

- 지정한 이벤트의 리스너를 추가하지만 한 번 실행한 후에는 자동으로 리스너가 제거된다.
### removeListener(eventName, listener)

- 지정한 이벤트에 대한 리스너를 제거한다.
### emit(eventName, `[,...args]`)

- 이벤트를 다른 쪽에 전달한다.

