- [[app.get()]] 방식일 때 [[콜백 함수(Callback Function)]] 안에 들어간다.

- 이때 [[콜백 함수(Callback Function)]]의 [[매개변수(parameter)]]가 [[ctx]]로 들어오므로 /:id와 같은 주소의 [[URL 파라미터]] 데이터를 코드로 추출할 때 사용한다.

## 문법

```js
const { id } = ctx.params;
```

- 위 코드와 같은 [[비구조화 할당]] 문법을 사용하여 [[URL 파라미터]]의 값을 추출할 수 있다.