- [[window]] [[객체(Object)]]가 브라우저 창이라고 한다면 document [[객체(Object)]]는 브라우저 내에서 콘텐트를 보여주는 웹 페이지 자체라고 할 수 있다.

- 웹 페이지에 존재하는 [[HTML(Hyper Text Markup Language)]] 요소에 접근하고자 할 때는 반드시 window.document [[객체(Object)]]부터 시작해야 한다.


## document 메서드

- document [[객체(Object)]]의 [[메서드(Method)]]들을 사용하면 다양한 방법으로 웹페이지 내의 태그들에 접근할 수 있다.
	- HTML 요소의 선택
	- HTML 요소의 생성
	- HTML 이벤트 핸들러 추가
	- HTML 객체 선택


### HTML 요소의 선택

- HTML 요소를 선택하기 위해 제공되는 메서드는 다음과 같다.

| 메서드                                    | 설명                            |
| -------------------------------------- | ----------------------------- |
| document.getElementsByTagName(태그이름)    | 해당 태그 이름의 요소를 모두 선택한다.        |
| document.getElementById(아이디)           | 해당 아이디의 요소를 선택한다.             |
| document.getElementsByClassName(클래스이름) | 해당 클래스에 속한 요소를 모두 선택한다.       |
| document.getElementsByName(name속성값)    | 해당 name 속성값을 가지는 요소를 모두 선택한다. |
| document.querySelectorAll(선택자)         | 해당 선택자로 선택되는 요소를 모두 선택한다.     |
- 이 중 document.[[querySelectorAll()]] [[메서드(Method)]]는 브라우저별로 지원 여부가 달라 사용시 확인 필요하다.

#### 하나의 요소에 접근하는 방법

```js
// 매개변수로 전달한 ID를 가진 태그를 반환
document.getElementById(요소아이디)

// 매개변수로 전달한 name 속성을 가진 태그를 반환
document.getElementByName(name속성값)

// 매개변수로 전달한 선택자에 맞는 첫 번째 태그를 반환
document.querySelector(선택자)
```

- 예시로 [[id]]="hello"이면 `#hello`로 [[class]]="hello"면 `.hello`로 선택이 된다.

#### 여러 개의 요소에 접근하는 방법

- 여러 개의 요소이므로 [[배열(Array)]]로 반환된다.

```js
// 매개변수로 전달한 태그이름을 가진 모든 태그들을 반환(배열)
document.getElementsByTagName(태그이름)

// 매개변수로 전달한 클래스 이름을 가진 모든 태그들을 반환(배열)
document.getElementsByClassName(클래스이름)

// 매개변수로 전달한 선택자에 맞는 모든 태그들을 반환(배열)
document.querySelectorAll(선택자)
```


### HTML 요소의 생성

- 새로운 HTML 요소를 생성하기 위해 제공되는 [[메서드(Method)]]는 다음과 같다.

| 메서드                                   | 설명                         |
| ------------------------------------- | -------------------------- |
| [[document.createElement()]] (HTML요소) | 지정된 HTML 요소를 생성한다.         |
| document.write(텍스트)                   | HTML 출력 스트림을 통해 텍스트를 출력한다. |



### HTML 이벤트 핸들러 추가

- HTML 요소에 [[이벤트(event)]] 핸들러를 추가하기 위해 제공되는 [[메서드(Method)]]는 다음과 같다.

| 메서드                                                         | 설명                                |
| ----------------------------------------------------------- | --------------------------------- |
| document.getElementById(아이디).onclick = function(){ 실행할 코드 } | 마우스 클릭 이벤트와 연결될 이벤트 핸들러 코드를 추가한다. |

### HTML 객체 선택

- HTML DOM Level 1은 1998년에 정의되어, HTML5에서도 계속 사용되고 있다.
- 그 후 2004년에는 HTML DOM Level 3가 새롭게 정의되어, Level 1과 같이 사용되고 있다.

- 이러한 [[HTML(Hyper Text Markup Language)]] [[DOM(Document Object Model)]]에서 제공하는 객체 집합(object collection)을 이용하면 HTML 객체를 손쉽게 선택할 수 있다.

| 객체 집합                        | 설명                                          |
| ---------------------------- | ------------------------------------------- |
| document.anchors             | name 속성을 가지는 `<a>`요소를 모두 반환함.               |
| document.applets             | applet 요소를 모두 반환함. (HTML5에서 제외됨)            |
| document.body                | `<body>` 요소를 반환한다.                          |
| document.cookie              | HTML 문서의 쿠키(cookie)를 반환한다.                  |
| document.domain              | HTML 문서가 위치한 서버의 도메인 네임(domain name)을 반환한다. |
| document.forms               | `<form>`요소를 모두 반환한다.                        |
| document.images              | `<img>`요소를 모두 반환한다.                         |
| document.links               | href 속성을 가지는 `<area>`요소와 `<a>`요소를 모두 반환한다.  |
| document.referrer            | 링크(linking)되어 있는 문서의 URI를 반환한다.             |
| document.title               | `<title>`요소를 반환한다.                          |
| document.URL                 | HTML 문서의 완전한 URL 주소를 반환한다.                  |
| document.baseURI             | HTML 문서의 절대 URI(absolute base URI)를 반환한다.   |
| document.doctype             | HTML 문서의 문서 타입(doctype)을 반환한다.              |
| document.documentElement     | `<html>`요소를 반환한다.                           |
| document.documentMode        | 웹 브라우저가 사용하고 있는 모드를 반환한다.                   |
| document.documentURI         | HTML 문서의 URI를 반환한다.                         |
| document.domConfig           | HTML DOM 설정을 반환한다.(더는 사용하지 않는다.)            |
| document.embeds              | `<embed>`요소를 모두 반환한다.                       |
| document.head                | `<head>`요소를 반환한다.                           |
| document.implementation      | HTML DOM 구현(implementation)을 반환한다.          |
| document.inputEncoding       | HTML 문서의 문자 인코딩(character set) 형식을 반환한다.    |
| document.lastModified        | HTML 문서의 마지막 갱신 날짜 및 시간을 반환한다.              |
| document.readyState          | HTML 문서의 로딩 상태(loading status)를 반환한다.       |
| document.scripts             | `<script>`요소를 모두 반환한다.                      |
| document.strictErrorChecking | 오류의 강제 검사 여부를 반환한다.                         |
