- @function은 [[@mixin]]과 마찬가지로 재사용을 위해 사용된다.
- 하지만 [[@mixin]]과의 차이점은 @mixin은 style markup을 반환하지만, @function은 @return을 통하여 값을 반환한다.


## 예시

```scss
$max-width: 980px; // 변수로 사용

@function columns($number: 1, $columns: 12) {
	@return $max-width * ($number / $columns)
}

.box_group {
	width: $max-width;
	
	.box1 {
		width: columns();  // 1
	}
	
	.box2 {
		width: columns(8);
	}
	
	.box3 {
		width: columns(3);
	}
}

/* 내가 정의한 함수 */
@function extract-red($color) {
/* 내장 함수 */
@return rgb(red($color), 0, 0);
}

div {
	color: extract-red(#D55A93);
}
```

- 위와 같이 @function은 [[@mixin]]처럼 [[@include]] 같은 별도의 지시어 없이 사용하기 때문에 생성한 함수와 내장 함수(Built-in Functions)의 이름이 충돌할 수 있다.
- 따라서 새롭게 생성한 함수에 별도의 접두어를 붙여 생성해주는 것을 권장한다.

- 내장 함수란, 응용 프로그램에 내장되어 있으며 최종 사용자가 액세스 할 수 있는 기능입니다.예를 들어, 대부분의 스프레드 시트 응용 프로그램은 행이나 열의 모든 셀을 추가하는 내장 SUM 함수를 지원한다.

- 예를 들어, 색의 빨강 성분을 가져오는 내장 함수로 이미 red()가 존재하는데, 같은 이름을 사용하여 함수를 정의하게되면 이름이 서로 충돌하기 때문에 별도의 접두어를 붙여 extract-red() 같은 이름으로 만들어 주면 된다.

```css
.box_group {
	/* 총 너비 */
	width: 980px;
}

.box_group .box1 {
	/* 총 너비의 약 8.3% */
	width: 81.66667px;
}

.box_group .box2 {
	/* 총 너비의 약 66.7% */
	width: 653.33333px;
}

.box_group .box3 {
	/* 총 너비의 25% */
	width: 245px;
}
```

혹은 모든 내장 함수의 이름을 다 알 수 없으니, 특별한 이름을 접두어로 사용하는 것도 방법이다. (ex. my-custom-func-red())

@function 역시 @mixin과 마찬가지로 인수에 대한 규칙이 존재한다.