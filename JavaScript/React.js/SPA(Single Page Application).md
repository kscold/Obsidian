- SPA(Single Page Application)는 한 개(Single)의 Page로 구성된 Application이다. 

- SPA는 [[웹(web)]] 에플리케이션에 필요한 모든 정적 리소스를 최초 한 번(HTML, CSS, JavaScript)에 다운로드하여 로딩한다.  
- 새로운 페이지 요청이 있을 때, 페이지 갱신에 필요한 데이터만 서버와 통신하여 페이지를 갱신한다.
- 즉, 첫 요청시 딱 한 페이지만 불러오고 페이지 이동 시 기존 페이지의 내부를 수정해서 보여주는 방식이다.
- 따라서 이를 클라이언트 관점에서 말하자면 최초 페이지를 로딩한 시점부터는 페이지 리로딩 없이 필요한 부분만 서버로 부터 받아서 화면을 갱신하는 것이다.  

- Angular, React, Vue 등 프론트엔드 기술들이 이를 적용하고 있다.

- 따라서 기본적으로는 SPA를 [[CSR(Client Side Rendering)]] 방식으로 렌더링으로 렌더링 한다. 
- 설정을 하면 리엑트에서도 [[SSR(Server Side Rendering)]]를 사용할 수 있다.

- 새로운 페이지 요청이 있을 때, 페이지 갱신에 필요한 데이터만 전달 받아서 페이지를 갱신한다.

- 이와 반대로 [[MPA(Multiple Page Application)]]는 여러 개(Single)의 Page로 구성된 Application이다. 

- 필요한 부분만 갱신하기 때문에 네이티브 앱에 가까운 자연스러운 페이지 이동과 사용자 경험(UX)을 제공할 수 있다.

[![SPA](https://i0.wp.com/hanamon.kr/wp-content/uploads/2021/03/SPA.png?resize=600%2C324&ssl=1)](https://hanamon.kr/spa-vs-mpa%ec%99%80-ssr-vs-csr-%ec%a0%95%eb%a6%ac/spa/)

### CSR 방식으로 만든 SPA 앱은 검색엔진최적화(SEO)가 어렵다.

- 일반적인 SPA 앱을 검색로봇 입장에서 보면 모든 페이지의 소스가 아래와 같이 보인다.

```html
<html>
<head>
	<title>Single Page Application</title>
	<link rel="stylesheet" href="app.css" type="text/css">
</head>

<body>
	<div id="app"></div>
	<script src="app.js"></script>
</body>
</html>
```
<html>

<head>

<title>Single Page Application</title>

<link rel="stylesheet" href="app.css" type="text/css">

</head>

<body>

<div id="app"></div>

<script src="app.js"></script>

</body>

</html>

 - 위의 예시를 보면 검색엔진이 색인을 할 만한 컨텐츠가 존재하지 않는다.(시멘틱 태그가 없다.)
<html>

<head>

<title>Single Page Application</title>

<link rel="stylesheet" href="app.css" type="text/css">

</head>

<body>

<div id="app"></div>

<script src="app.js"></script>

</body>

</html>

## SPA 장점

1. 자연스러운 사용자 경험을 제공한다.(UX가 좋다.)
    - 전체 페이지를 업데이트 할 필요가 없기 때문에 빠르고 ‘깜빡’ 거림이 없다.([[컴포넌트(component)]] 단위로 렌더링 가능)

2. 필요한 리소스만 부분적으로 로딩한다.(성능이 좋다.)
    - SPA의 Application은 서버에게 정적리소스를 한 번만 요청한다.
    - 그리고 받은 데이터는 전부 저장해놓는다.(캐싱)

3. 서버의 템플릿 연산을 클라이언트로 분산한다.(성능이 좋다.)

4. [[컴포넌트(component)]]별 개발 용이하다. (생산성이 좋다.)

5. 모바일 앱 개발을 염두에 둔다면 동일한 API를 사용하도록 설계 가능하다. (생산성이 좋다.)

## SPA 단점

1. 자바스크립트 파일을 번들링해서 한 번에 받기 때문에 초기 구동 속도가 느리다.([[웹팩(webpack)]]의 code splitting으로 해결 가능하다.)

2. 검색엔진최적화(SEO)가 어렵다.(SSR로 해결 가능하다.)

3. 보안 이슈(프론트엔드에 비즈니스 로직 최소화)
    - SSR에서는 사용자에 대한 정보를 서버측에서 세션으로 관리를 하지만 CSR 방식에서는 클라이언트측의 쿠키말고는 사용자에 대한 정보를 저장할 공간이 마땅치 않다.