- querySelector()는 특정 [[name]], [[id]] ,[[class]]를 제한하지 않고 css선택자를 사용하여 요소를 찾는다.

- 같은 [[id]] 또는 [[class]] 일 경우 스크립트의 최상단 요소만 로직에 포함한다.

```js
querySelector(#id) // id 값 id를 가진 요소를 찾음
querySelector(.class) // class 값 class를 가진 요소를 찾음
```