- mixin(믹스인)은 함수와 비슷한 동작을 하는 문법이며 CSS 스타일 시트에서 반복적으로 재사용할 CSS 스타일 그룹 선언을 정의하는 기능을 한다.

- 따라서 스타일 정의 앞에 `@mixin`이 붙게 되면 해당 스타일 시트 내부에서 얼마든지 재사용이 가능하다.

- 단순하게 CSS 그룹으로 정의하여 적용할 수 있지만 인수를 활용하게 되면, 반복되는 CSS 속성을 한 개의 mixin(믹스인) 정의를 가지고 다양한 CSS 스타일을 만들어 낼 수 있다.

## 문법

- @mixin을 정의해 만든 CSS 스타일을 @include 이용하여 참조해서 재사용할 수 있다.
- @mixin, @include 옆에 사용되는 이름은 selector가 아닌 함수 이름처럼 mixin 이름이다.

```scss
//@mixin - 스타일 정의
@mixin 믹스인 이름 {
	//CSS 스타일 내용
}

//@include - 믹스인 호출
@include 믹스인 이름
```


- 정의할 때에는 `@mixin 믹스인 이름 { CSS 스타일 }` 형식으로 정의한다.
- 호출할 때에는 [[@include]] `믹스인 이름` 형식으로 사용한다.

```css
@mixin reusable-style {
  color: red;
  margin: 0;
}

@include reuable-style
```

- mixin 내부에서 `&`나 또 다른 셀렉터를 포함할 수도 있다.

## @mixin의 인자

- `@mixin`은 마치 함수처럼 인자([[$(Argument)]])를 가질 수 있다.
- 인자의 선언과 사용도 마치 함수처럼 사용할 수 있다.

```scss
@mixin mixin-with-args($props, $num) {
	#{$props}: $num;
}

.box1 {
	@include mixin-with-args(width, 100px);
}
```

```css
// 컴파일 결과
.box {
	width: 100px;
}
```

## Content Blocks

- mixin에 `@content`를 선언하면 해당 구역에 새로운 스타일 블록을 넣을 수 있게 된다.
- 오버라이딩과 굉장히 유사하다.

```scss
@mixin box-style() {
	width: 50px;
	height: 50px;
	@content;
}

/* @content 기능을 이용하지 않은 box1 */
.box1 {
	@include box-style();
}

/* @content 기능을 이용한 box2 */
.box2 {
	@include box-style() {
		background-color: black;
	};
}
```

```css
// 컴파일 결과
/* @content 기능을 이용하지 않은 box1 */
.box1 {
	width: 50px;
	height: 50px;
}

/* @content 기능을 이용한 box2 */
.box2 {
	width: 50px;
	height: 50px;
	background-color: black;
}
```

- `@content`블록에 전달된 스타일 블록은 mixin에 포함된 것 처럼 보이지만 `@include`된 스타일 블록의 스코프(Global)을 따라간다.

```css
$color: black;

@mixin box-style($color) {
    color: $color;
    @content;
}

div {
    @include box-style(blue) {
        color: $color;
    };
}
```


```css
// 컴파일 결과
div {
	color: blue;
	/*
	content 내부의 $color 변수는 전역 변수값을 따라서 black으로 컴파일됨
	즉, @content 내의 변수는 mixin이 아닌,div에서 스코프를 가진다는 것을 의미함
	*/
	color: black;
}
```