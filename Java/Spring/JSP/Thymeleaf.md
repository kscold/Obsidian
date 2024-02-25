-  타임리프는 [[컨트롤러(Controller)]]가 전달하는 데이터를 이용해 동적으로 화면을 만들어주는 역할을 하는 [[뷰(View)]] [[템플릿(Template)]] 엔진이다.

-  타임리프는 순수 HTML을 유지하기 때문에 Natural Template(내추럴 템플릿)이라고도 불린다. 
- 서버를 가동하지 않으면 순수 HTML을, 서버를 구동하여 뷰 템플릿을 거쳐 동적으로 변경된 HTML을 확인할 수 있다.

- [[JSP]]같은 경우는 서버를 구동하지 않고 해당 파일을 열게 되면 JSP 소스코드와 HTML이 섞여있어서 정상적인 확인이 불가능했다. 
- 즉 오직 서버를 통해서 JSP을 열어야 JSP 파일을 확인할 수 있었다. 
- 반면에 타임리프는 화면 구성을 서버 가동없이 쉽게 파악할 수 있어 개발에 수정할 때마다 서버 재가동이 필요 없어지기 때문에 개발에 용이해진다.


## build.gradle 설정

```
implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
```

 - 타임리프 문법은 다음과 같은 규칙이 있습니다.
- html 태그의 속성에 [th:]라는 접두사가 붙은 속성을 사용해 thymeleaf 문법을 사용할 수 있다.

## 타임리프 문법

### th:fragment

```jsp
<h1 th:fragment="header">Header</h1>`
```

- 위의 코드와 같이 문법은 th:fragment=”템플릿 조각 이름”형식을 사용하여 header [[템플릿(Template)]] 조각을 만든 것이다.
- th:fragment는 템플릿 조각을 선언하고 조각의 네이밍을 붙인다.

### th:insert

```jsp
<div th:insert="~{layouts/header :: header}"></div>
<div th:insert="layouts/header :: header"></div>
```
### th:replace

```jsp
<div th:replace="~{layouts/header :: header}"></div>
<div th:replace="layouts/header :: header"></div>
```

- insert와 replace 사용문법은 동일하다.
- ~{경로 :: 템플릿조각 이름}와 같이 사용하면 된다.
- 템플릿 조각 이름은 header.html에 th:fragment=”header”의 header를 의미한다.

- 최신 타임리프 문법에서는 "경로 :: 템플릿조각 이름"으로 사용할 수 있다.
- 경로 작성에 주의해야되는데, Thymeleaf에 특정한 설정이 없다면 기본 default 최상위 경로는 templates/로 시작한다.

```jsp
<!DOCTYPE html>  
<html lang="ko" xml:th="http://www.thymeleaf.org"> <!-- 타임리프 설정을 위해 명시 -->  
<head>  
    <meta charset="UTF-8">  
    <title>Title</title>  
</head>  
<body>  
<header>  
    header 삽입부  
    <hr>  
</header>  
</body>  
</html>
```