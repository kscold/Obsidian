- componentDidMount()은 [[컴포넌트(Component)]]를 만들고, 첫 렌더링을 다 마친 후 실행한다.

- 이 안에서 다른 자바스크립트 라이브러리 또는 프레임워크 함수를 호출하거나 [[이벤트(event)]] 등록, [[setTimeout()]], setInterval, 네트워크 요청 같은 [[비동기(asynchronous)]] 작업을 처리하면 된다.

- [[useEffect()]]의 `[]` 빈 의존성 배열이라고 생각하면 된다.

## 문법

```js
componentDidMount() { ... }
```