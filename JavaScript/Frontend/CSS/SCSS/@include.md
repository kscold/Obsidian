- `@include`는 [[@mixin]]을 사용하기 위해 선언하는 구문이다.
- 셀렉터를 지정하고 그 내부에 `@include`를 통해 mixin을 불러오면 된다.

```scss
div {
  @include resuable-style;
}
```

```css
// 컴파일 결과
div {
	color: red;
	margin: 0;
}
```