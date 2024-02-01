- mixin(믹스인)은 함수와 비슷한 동작을 하는 문법이며 CSS 스타일 시트에서 반복적으로 재사용할 CSS 스타일 그룹 선언을 정의하는 기능을 한다.

- 단순하게 CSS 그룹으로 정의하여 적용할 수 있지만 인수를 활용하게 되면, 반복되는 CSS 속성을 한 개의 mixin(믹스인) 정의를 가지고 다양한 CSS 스타일을 만들어 낼 수 있다.

- 정의할 때에는 `@mixin 믹스인 이름 { CSS 스타일 }` 형식으로 정의한다.
- 호출할 때에는 `@include 믹스인 이름` 형식으로 사용한다.

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

## @mixin

- `@mixin`은 재사용이 가능한 스타일을 정의하는데 사용되는 선언이다. 
- 스타일 정의 앞에 `@mixin`이 붙게 되면 해당 스타일 시트 내부에서 얼마든지 재사용이 가능하다.

```css
@mixin reusable-style {
  color: red;
  margin: 0;
}
```

- mixin 내부에서 `&`나 또 다른 셀렉터를 포함할 수도 있다.

## @mixin의 인자

- `@mixin`은 마치 함수처럼 인자(Argument)를 가질 수 있다.
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

### 인자 기본값

- @mixin의 인자에도 기본값을 할당할 수 있다.
- 기본값은 인자가 전달되지 않았을 경우 사용된다.

- 인자는 콜론`:`을 사용하여 `$매개변수:값`의 형태로 표기한다.

```scss
@mixin default-args($props: width, $num: 100px) {
	#{$props}: $num;
}
```

- 인자에 기본값을 할당하면, 인수는 optional하게 만들 수 있다.

```scss
@mixin img-position($img, $x: 10%, $y: 10%) {
	background: {
	    image: $img;
	    position: $x, $y;
	}
}

.img {
	@include img-position(url("./a.img"), 20%);
}
```

```css
// 컴파일 결과
.img {
	background-image: url("./a.img");
	background-position: 20% 10%;
}
```

- mixin의 세번째 인자인 `$y`가 optional 해졌다.
- 그 결과로 background-position의 y축이 10%로 자동삽입 되었다.

### 키워드 인자

- 키워드 인자는 인자를 명시적으로 전달할 수 있는 인자이다. 
- 문법은 기본값 작성하듯이 하는데, 인자를 전달할 때 사용한다는 차이점을 기억하자.
- 처음 접근하면 자칫 헷갈릴 수 있다.

- 키워드 인자의 장점은 기존 인자는 선언 순서대로 지정했던 것에 비해서 인자를 전달하는 순서에 구애받지 않는다는 장점**이 있다.

```scss
@mixin box-style($width: 100px, $height: 50px, $color: red) {
	width: $width;
	height: $heigth;
	background-color: $color; 
}

/* mixin을 include하는 부분을 주목 */
.box {
	@include box-style($color: blue, $height: 100px);
}
```


```css
// 컴파일 결과
.box {
	width: 100px;
	height: 100px;
    background-color: blue;
}
```

 - 키워드 인자는 인자의 이름으로 인수를 전달하기 때문에 mixin 인수의 이름을 변경할 때는 반드시 주의해야 한다.
 
### 가변 인자(임의 길이 인자)

- 인자의 갯수가 고정되어있지 않은 경우도 있다.
- 이럴땐 인자 뒤에 `...`을 붙임으로써 인자를 가변 인자로 만들어서 사용한다.

```scss
@mixin margin-size($size...) {
	margin: $size; 
}

.box1 {
	@include margin-size(0px, 0px, 0px, 0px);
}
.box2 {
	@include margin-size(0px, 0px, 0px);
}
.box3 {
	@include margin-size(0px, 0px);
}
.box4 {
	@include margin-size(0px);
}
```


```css
// 컴파일 결과
.box1 {
	margin: 0 0 0 0;
}

.box2 {
	margin: 0 0 0;
}

.box3 {
	margin: 0 0;
}

.box4 {
	margin: 0;
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