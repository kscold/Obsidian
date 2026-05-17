- `<table>`은 표를 만드는 태그이다.
- 홈페이지의 전체 구성(layout)을 만드는 데도 사용될 수 있다.
- 하지만 원래 table 태그는,  위 그림처럼 데이터를 담고 있는 표를 만들기 위한 목적이다.

- 그래서 홈페이지 전체 구성이나 배치를 위해선, [[<div>]]태그를 사용하는 것이 좋으며 

![](https://t1.daumcdn.net/cfile/tistory/25113E405316DAA606)



![](https://t1.daumcdn.net/cfile/tistory/215034405316DAA521)

- html 기초 태그에 대해서도 썼듯, `<table>` 역시 `</table>`과 쌍을 이루어 사용한다.
- `<table>`은 각각 셀로 만드는데  header cell과 standard cell로 나누어진다.
- `<th>`를 header cell이라 부르며 나머지를 standard cell로 부른다.

## `<table>`과 같이 쓰이는 태그

### `<th>내용</th>`

- table head 약자로, 표의 제목을 쓰는 역할이다.
- 기본값은 굵은 글씨체에, 중앙 정렬이다.
### [[<Tr>]]`내용 </tr>` 

- table row 약자로, 가로줄을 만드는 역할이다. 
- 기본값은 보통 글씨체에, 왼쪽 정렬한다.
### `<td>내용</td>`  

- table data 약자로, 셀을 만드는 역할이다.
- 기본값은 보통 글씨체에, 왼쪽 정렬이다.

```html
<html>  
<head>  
</head>  
<body>

<table border="1">  
  <tr>  
    <th>번호</th>  
    <th>이름</th>  
  </tr>  
  <tr>  
    <td>1</td>  
    <td>김영희</td>  
  </tr>  
  <tr>  
    <td>2</td>  
    <td>김순자</td>  
  </tr>  
</table>

</body>  
</html>
```

- 아래 이미지처럼 표시된다.
- 번호, 이름 부분은 header cell로 굵은 글씨체에 중앙 정렬되었고, 1 김영희, 2 김순자는 standard cell로 보통 글씨체에 왼쪽 정렬되었다.

![](https://t1.daumcdn.net/cfile/tistory/27443A4C5316E0BF0E)

  
## `<table>` 태그의 속성

- table 속성은 colspan, rowspan, border, bgcolor, width, height, cellpadding, cellspacing, align, valign 등이 있다.

### colspan 

- column span 약자로, 셀(가로줄)을 합치는 개수를 지정한다.

```html
<table border="1" >
	<tr>
		<td colspan="2">병아리반</td>  
	</tr>  
	<tr>
		<td> 1</td>
		<td>김영희</td>  
	</tr>  
	<tr>
		<td> 2</td> 
		<td>김순자</td> 
	</tr>  
</table>
```

- 밑의 이미지는 colspan="2" 로 설정했을 때 화면이다.

![](https://t1.daumcdn.net/cfile/tistory/2755F34C5316E0BF07)

### rowspan

- row span이란 뜻으로 셀(세로줄)을 합치는 개수를 지정한다.

```html
<table border="1" >
	<tr>
		<td rowspan="3">병아리반</td>  
	<td>김순자</td>  
	</tr>  
	<tr>  
		<td>김영희</td>
	</tr>
	<tr>  
		<td>이철수</td>  
	</tr>  
</table>
```

- 밑의 이미지는 rowspan="3" 으로 설정했을 때 화면이다.

![](https://t1.daumcdn.net/cfile/tistory/226CD84C5316E0BF32)

### border

- 테이블 경계선 굵기를 지정한다.
- 예시) border="10"

### width 

- 너비를 지정한다.
- 픽셀이나, %를 사용한다.

### height 

- 높이를 지정한다.
- 픽셀이나, %를 사용한다.

- 아래 이미지는border ="10" width="100%" 설정한 화면이다.
- width와 height는 픽셀로 설정(width="50px")하거나, %로 설정할 수 있다.

![](https://t1.daumcdn.net/cfile/tistory/214F744E5316E27329)

### cellpadding

- 셀과 경계선 사이의 여백를 설정한다.
- 아래 이미지는 cellpadding="10"으로 지정한 화면이다.

![](https://t1.daumcdn.net/cfile/tistory/24674C4A5316E14606)

### cellspacing

- 셀과 셀 사이의 여백이다.
- 아래 이미지는 cellspacing="10"으로 지정한 화면이다.

![](https://t1.daumcdn.net/cfile/tistory/21201E4A5316E1461E)

### align 

- 셀의 가로줄을   오른쪽(right), 왼쪽(left), 중앙(center) 등으로 정렬한다.
- 아래 이미지는 align="left" 설정한 화면이다.

![](https://t1.daumcdn.net/cfile/tistory/214F744E5316E27329)

### valign 

- 셀의 세로줄의 위(top), 중앙(middle), 아래(bottom)를 정렬한다.
- 아래 이미지는 valign="middle" 설정한 화면이다.

![](https://t1.daumcdn.net/cfile/tistory/231F544B5316E28810)

### bgcolor

- 배경색을 지정한다.
- bgcolor 속성값으로는 색상 이름(예:green)이나, 색상코드(#ff0000) 등을 사용할 수 있다.
- 아래 이미지는 bgcolor으로 지정한 화면이다.

```html
<table bgcolor="green"> 
```

![](https://t1.daumcdn.net/cfile/tistory/24395F4653171F800E)

### bordercolor

- 경계선 색깔읉 지정한다..
- 아래 이미지는 bordercolor으로 지정한 화면이다.

```html
<table border="1" bordercolor="red">
```

![](https://t1.daumcdn.net/cfile/tistory/252A9A47531824CD0E)

  

  

  

  

위에서 <table> 태그의 여러 속성들에 대해 살펴봤는데

위 속성 들 중에는 HTML5에서 지원되지 않는 속성도 있습니다.

  

HTML5에서 지원되지 않는 속성 :  
align, bgcolor, border, cellpadding, cellspacing, frame, rules, summary, width 

  

그래서 위의 속성들은 css를 통해 지정하는 것이 바람직합니다(아래 링크 참조)