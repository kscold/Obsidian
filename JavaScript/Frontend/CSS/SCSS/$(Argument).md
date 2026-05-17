- [[@mixin]]의 인자에도 기본값을 할당할 수 있다.

- 기본값은 인자가 전달되지 않았을 경우 사용된다.


## 문법

- 인자는 콜론 `:`을 사용하여 `$매개변수:값`의 형태로 표기한다.

```scss
@mixin default-args($props: width, $num: 100px) {
	#{$props}: $num;
}
```

- 인자에 기본값을 할당하면, 인자는 optional하게 만들 수 있다.

```scss
@mixin img-position($img, $x: 10%, $y: 10%) {
	background: {
	    image: $img;
	    position: $x, $y;
	}
}

.img {
	@include img-position(url("./a.img"), 20%); // mixin 호출
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

## 키워드 인자

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
 
### 가변 인자(임의 길이 인자

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
