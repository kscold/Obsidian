-  scss에서 [[객체(Object)]] 형식처럼 map-get를 사용하여 key와 value를 매칭해서 사용하는 방식이다.
- 이렇게 한 변수안에 [[배열(Array)]]처럼 여러 값을 넣을 수 있다.


## 문법

```scss
$variable: (
    key : value,
);

body {
	background-color: map-get($map: $variable, $key: key);	
}
```

## 예시

```scss
$color: (
    // 이름: 값, 이름: 값, 이름:값
    font-primary : #2d3436,
    font-secondary : #636e72,
    font-focus: #0984e3,
    bgc-primary: #dfe6e9,
    bgc-secondary: #b2bec3,
);
```


```scss
body {
    background-color: map-get($map: $color, $key: bgc-primary);
    margin: 0;
}
```
