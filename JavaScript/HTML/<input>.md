- 바닐라 input [[키워드(Keyword)]]는 닫는 태그가 없다.
- type, value, name  등등 속성이 있다.
- 리액트는 역시 태그를 열었으면 닫아줘야 한다.

## type 
- input의 타입을 지정해주는 속성으로 기본 타입은 text이다.

1.Text
`<input type = "text">`  
- 텍스트로 입력받을 수 있음

2.Radio 
`<input type = "radio">` 
- 단일 선택 체크박스

3.Checkbox
 `<input type = "checkbox">`
 - 다중 선택 체크박스

4.Submit
`<input type = "submit">` 
- 폼 양식을 서버에 전송

5.Button
`<input type = "button">`
- 버튼

6.Reset
`<input type = "reset">` 
- 폼 초기화

7.File
`<input type = "file">` 
- 파일 업로드

8.date
`<input type = "date">` 
- 날짜 정보 입력

9.number
`<input type = "number">`
-  숫자 정보 입력

10.Email
`<input type = "email">` 
- 이메일 형식 입력

11.Color
`<input type = "color">`
- 색상 입력

12.Range
`<input type = "range" min="0" max="50">` 
- 범위 입력

13.Search
`<input type = "search">`
- 검색 인풋 요소


## name
- name 속성은 `<input>` 태그의 이름을 명시한다.
- name 속성은 폼([[<form>]])이 제출된 후 서버에서 폼 데이터(form data)를 참조하기 위해 사용되거나, 자바스크립트에서 태그를 참조하기 위해 사용된다.

## placeholder 
- 입력 필드에 사용자가 적절한 값을 입력할 수 있도록 도와주는 짧은 도움말을 명시한다.
- placeholder 속성이 제대로 동작하는 type 속성은 email, password, search, tel, text, url가 있다.

## vlaue
- 사용자 세팅 초기값
- value 속성은 `<input>` 태그의 초깃값(value)을 명시한다.
- value 속성은 `<input>` 태그의 type 속성값에 따라 다른 용도로 사용된다.
	- “button”, “reset”, “submit” : 버튼 내의 텍스트를 정의한다.
	- “hidden”, “password”, “text” : 입력 필드의 초깃값을 정의한다.
	- “checkbox”, “image”, “radio” : 해당 입력 필드를 선택 시 서버로 제출되는 값을 정의한다.
	- 또한, `<input>` 요소의 type 속성값이 “file”인 경우에는 value 속성을 사용할 수 없다.
 
 
  ## [[onChange]]
  - `<input>` 태그의 [[이벤트(event)]]를 받아 event.target.value로 input창의 값을 저장할 수 있다.
- 이벤트를 여러개 다뤄야할 때는 [e.target.name] : e.target.value를 사용해 받아주면 더 좋을 수도 있다.

```jsx
	state = {
		username: '',
		message: '',
	};

	  
	handleChange = (e) => {
		// 화살표함수로 선언하면 메서드 바인딩을 생략할 수 있다.
		this.setState({
			[e.target.name]: e.target.value,
		});
	};
	
```