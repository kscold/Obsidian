- 특정 선택자가 다른 선택자의 모든 스타일을 가져야 하는 경우가 있다.
- 이경우 @extend를 활용하여 객체지향의 상속처럼 활용할 수 있다.

## 문법

```scss
@extend 선택자;
```

## 에시

```scss
.btn {
	padding: 10px;
	margin: 10px;
	background: blue;
}

.btn-danger {
	@extend .btn;
	background: red;
}
```


```css
// 컴파일 결과
.btn, .btn-danger {
  padding: 10px;
  margin: 10px;
  background: blue;
}

.btn-danger {
  background: red;
}
```