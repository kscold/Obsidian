- 미디어 쿼리는 CSS에서 나온 기술로 특정 조건이 성립됐을 때 CSS 속성을 적용할 수 있게 해주는 문법이다.

- 위에서 말한 특정 조건이란 화면의 넓이나 높이와 같이 브라우저가 어떤 기기에서 실행되고 있는지를 파악해서 기기별로 서로 다른 스타일을 제공하는 방법을 선택하는데, 이것을 반응형 프로그래밍이라고 한다.

## 문법

![[Pasted image 20240201231316.png]]

### only | not 

- only는 뒤의 조건만 적용한다는 것이고 not은 뒤의 조건을 제외하고 적용한다는 뜻이다.
### 미디어 타입

- all : 모든 미디어 타입
- aural : 음성장치
- braille : 점자 장치
- print : 인쇄 용도
- projection : 프로젝터
- screen : 컴퓨터 스크린
- tv : 음성과 영상이 동시에 출력되는 장치
- mbrossed : 페이지에 인쇄된 점자 표시장치

- 많이 쓰는 타입은 screen과 print이다.  

- 여러 미디어 타입을 적용하고 싶으면 아래와 같이 쉼표로 구분지어서 사용하면 여러개의 타입을 적용할 수 있다.

```css
@media only screen, print and(width: 1000px){...]
```

### 속성

- width : 페이지의 가로길이이다.
- height : 페이지의 세로길이이다.
- device-width : 단말기의 가로길이이다.
- device-height : 단말기의 세로길이이다.
- aspect-ratio : width와 height의 비율을 판단이다.
- device-aspect-ratio : 단말기의 물리적인 화면 비율이다.
- color-index : 단말기에서 사용하는 최대 색상수이다.
- resolution : 지원하는 해상도를 판단, 화면의 크기는 같지만 지원하는 해상도가 다른 기기의 경우 판단할때 쓰면 좋다.

## 미디어 쿼리 사용 방법

- 밑의 예시 코드는 screen 영역이 768px 전 까지의 width에서 속성이 적용된다.

```css
@media screen and (max-width: 768px) {
  color: white;
  background-color: black;
}
```

- 밑의 예시 코드는 웹에 접근한 기기가 screen일때 가로길이가 100px미만이면 배경을 파랑색으로 200px이상이면 초록색으로 100px에서 200px사이이면 빨강색으로 적용된다는 것이다.

```html
<style>
	div{
		width: 100%;
		height: 100%;
	}
	
	@media screen and(max-width:100px){
	    div{background-color: blue;}
	}
	
	@media screen and(min-width:100px) and(max-width:200px){
	    div{background-color: red;} 
	}
	
	@media screen and(min-width:200px){
	    div{background-color: green}
	}
</style>

<body>
	<div></div>
<body>
```

## SCSS에서의 사용

- 밑의 코드는 scss만의 문법([[@mixin]] 과 변수$)을 사용하여 브레이크포인트를 선언하는 예시이다.

```scss
/* Responsive layout, breakpoint */

$mobile-max: 360px; // 변수로 선언
$tablet-max: 1023px;
$desktop-min: 1024px;

@mixin mobile{
	@media (max-width: $mobile-max){
	    @content;
	}
}

@mixin tablet{
	@media (max-width: $tablet-max) and (min-width: $mobile-max){
		@content;
	}
}

@mixin desktop{
	@media (min-width: $desktop-min){
		@content;
	}
}
```

