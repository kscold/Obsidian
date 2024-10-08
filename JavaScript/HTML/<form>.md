- 주로 [[<input>]] 태그와 [[<button>]] 묶어 사용할 수 있다.
- 자동적으로 백엔드 DTO를 일으키기 때문에 [[preventDefault()]]를 사용하여 방지할 수 있다.

## 속성

### [[onChange]]

- 유저의 입력이 어떤 식으로든 변경될 때 발생하는 것으로, form의 입력 요소에 변경이 생기면 발생한다. (예) 유저가 텍스트를 입력, 옵션 선택, 확인란의 선택 취소 등등이 있다.)
### [[onSubmit]]

- form을 제출할 때 [[이벤트(event)]]가 발생한다.
- onSubmit [[이벤트(event)]]는 브라우저를 새로고침을 시킨다.
- onSubmit 이벤트의 경우 "Enter" 키를 눌렀을 때도 발생하기 때문에 [[<input>]] 태그에 onKeyPress를 굳이 추가하지 않아도 된다.

### onInput

- 요소의 값이 변경될 때 발생한다.(권장하지 않는 옵션이다.)
- [[onChange]]는 이벤트가 포커스를 잃을 때 뿐만 아니라 키를 입력할 때 마다 발생한다. 
- onInput은 [[DOM(Document Object Model)]]의 onInput 이벤트를 감싼 것으로, 변경할 때마다 발생한다.


### enctype="multipart/form-data"

- 파일을 업로드 하는 HTML form 태그를 이용한다.
- enctype은 인코딩 타입이며, 영어 알파벳 이외의 문자들도 분명히 있을 이진파일을 POST로 보내기 위해서는 `multipart/form-data`를 이용해야 한다.

```html
<form method="POST" , action="" , enctype="multipart/form-data">
	<label for="myfile">Choose A file to Upload</label>
	<input type="file" id="myfile" />
</form>
```

- [[Express]]에서 파일 처리를 담당하는 [[미들웨어(Middleware)]]인 [[multer]]를 사용할 때 주로 사용한다.
