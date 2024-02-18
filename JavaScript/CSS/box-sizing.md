- `box-sizing`은 박스의 크기를 어떤 것을 기준으로 계산할지를 정하는 속성이다.

- 기본값 : `content-box`
- 상속 : No
- 애니메이션 : No
- 버전 : CSS Level 3

## 문법

box-sizing: content-box | border-box | initial | inherit

- `content-box` : 콘텐트 영역을 기준으로 크기를 정한다.
- `border-box` : 테두리를 기준으로 크기를 정한다.
- `initial` : 기본값으로 설정한다.
- `inherit` : 부모 요소의 속성값을 상속받는다.


## 예제

```html
<!doctype html>
<html lang="ko">

	<head>
	
		<meta charset="utf-8">
	
		<title>CSS</title>
	
		<style>
	
			body {
				margin: 0px;
			}
			
			div {
				margin: 20px;
				padding: 20px;
				border: 20px solid #dddddd;
				width: 500px;
			}
	
			.cb {
				box-sizing: content-box;
			}
			
			.bb {
				box-sizing: border-box;
			}
		
		</style>
	</head>
	
	<body>
		<div class="cb">
			<p>content-box</p>
		</div>
	
		<div class="bb">
			<p>border-box</p>
		</div>
	</body>
	
</html>
```


![](https://www.codingfactory.net/wp-content/uploads/css-property-box-sizing-01.png)

둘 다 `width` 값을 `500px`로 정하였으나, `box-sizing` 속성값에 따라 크기기 달라진다.

첫 번째 박스는 콘텐트 영역이 `500px`이고 테두리를 포함한 크기는 `580px`이다.

![](https://www.codingfactory.net/wp-content/uploads/css-property-box-sizing-02.png)

두 번째 박스는 테두리를 포함한 크기가 `500px`이고, 콘텐트 영역의 크기는 `420px`이다.

![](https://www.codingfactory.net/wp-content/uploads/css-property-box-sizing-03.png)