- 자바스크립트의 Math [[클래스(Class)]]([[객체(Object)]])는 랜덤 [[메서드(Method)]]를 포함하여 다양한 계산 [[메서드(Method)]]를 포함하고 있다.


## Math [[메서드(Method)]] 사용


```js
// Math Object
val = Math.E; // 자연 e
val = Math.PI; // pi

val = Math.round(2.4); // 가장 가까운 정수로 만듬, 파이썬처럼 소수점 자르기 안됨
val = Math.ceil(2.4); // 3
val = Math.floor(2.8); // 2
val = Math.abs(-5); //5
val = Math.min(2, 3, 4, 5, 6, 7, 8, -1); // -1
val = Math.max(2, 3, 4, 5, 6, 7, 8); // 8

val = Math.random(); // 0부터 1까지 랜덤 소수점

// 1 - 3
val = Math.floor(Math.random() * 3 + 1);
```