- jsp는 html 문서에 내부적으로 자바 문법을 사용할 수 있게 하는 웹페이지 스크립트 언어이다.
- java언어 기반으로 동적인 웹 페이지([[WAS(Web Application Sever)]])를 구성할 수 있다.

- .jsp 확장자로 html코드 안에 자바 코드가 들어가는 방식이다.

- 서블릿을 보완하고 기술을 확장한 스크립트 방식 표준이다. ([[Servlet]](서블릿)의 모든 기능 + 추가기능)

- presentation([[뷰(View)]])에 좋다.

![[Pasted image 20231101232712.png]]

##  JSP의 동작과정

- jsp파일은 javax.servlet.http.[[HttpServlet]] 클래스를 상속받은 Java 소스 코드로 변환한 다음 컴파일되어 실행된다.

![[Pasted image 20231101232719.png]]

- 아래 이미지와 같이  <%= %>으로 java코드를 html파일에 삽입하는 방식으로 사용 가능하다.
![[Pasted image 20231101232823.png]]

- 선언태그와 주석 태그 <%= %> / 주석은 -- 하이폰 두개임으로 헷갈림에 유의해야 한다.

![[Pasted image 20231101232952.png]]


## 스크립트릿 태그(<% %>)와 표현식 태그

- 스크립트릿 태그를 자연스럽게 가장많이 쓴다.  

![](https://velog.velcdn.com/images%2Fqlgks1%2Fpost%2F1b97d7b0-8677-4fa0-91be-3541fd07b7c6%2Fimage.png)

## JSP 지시어

- page는 거의 동일하다.(보통 default)

- page import="java.util.ArrayList" 와 같이, java코드를 넣을때 필요한 라이브러리 import 할때도 page를 사용한다.

- include는 외부 다른 파일 가져올때 많이 사용한다.
- header / footer 등과 같은 반복되는 덩어리들에 대한 처리로 include를 많이 사용한다.

- taglib은 외부 라이브러리(jQuery 등)를 import할때 사용한다.

![](https://velog.velcdn.com/images%2Fqlgks1%2Fpost%2Fc24e16a8-54ae-48da-8ab4-559b673556a0%2Fimage.png)

## request와 response

![](https://velog.velcdn.com/images%2Fqlgks1%2Fpost%2F27818b7f-027a-450b-87ac-ee308805a9aa%2Fimage.png)

- 요청은 getParameterValues 메서드로 [[배열(Array)]] 형태로 받아올 수 있다.

- 응답은 현제 요청 페이지에서 다른 페이지로 [[Redirect]] 하는 코드이다.

![](https://velog.velcdn.com/images%2Fqlgks1%2Fpost%2Feebd26cc-e68c-4e6c-b397-68c08cf290c5%2Fimage.png)


- 밑의 이미지는 jsp에서 request를 통해 소통하는 예시 코드이다.

![](https://velog.velcdn.com/images%2Fqlgks1%2Fpost%2F257e762e-eec0-4acb-bab4-4a4cb18f1bce%2Fimage.png)

- action은 request가 갈 target url이다.
- 이미지에서는 생략되었지만 jsp파일 경로 (mSignUp.jsp)를 넣어주야 한다.
- 메서드는 http request의 형태이다.

- 위 html [[<form>]] 태그로 쿼리스트링 형태로 action url에 Request가 갈 것이다.

![](https://velog.velcdn.com/images%2Fqlgks1%2Fpost%2Ffb3b7c0c-1aa9-4916-b98c-b51f5ba6622d%2Fimage.png)

- jsp파일에서 request가 들어온 (query string value 기반) 데이를 기반으로 page를 render한다!

## [[내장 객체]]

- request, response 외 jsp에서 기본적으로 제공하는 다른 내장 객체는 다음과 같다.

### 1. config객체

- web.xml에서 init param와 같은 xml태그를 이용해 -> jsp에서 getInitParameter() 메소드를 이용해 confing된, (미리 정의된) 파라미터를 이용할 수 있다.
- target servelt xml 태그 안에 위와 같은 추가 정보 xml이 정의 되어야 한다!  
    ![](https://velog.velcdn.com/images%2Fqlgks1%2Fpost%2Fc57b4fbd-786f-4140-a1ba-ba5c4ed89b59%2Fimage.png)

### 2. application 객체

- config에서 [[스코프(Scope)]]가 확장되었다고 생각하면 편하다.
- global 영역에서, application 전역에서, 미리 정의한 이 객체에 대해 접근할 수 있다.

- `getServletContext().setAttribute("key", "value");`와 같이 정의되어 있는 것을 update하는 형식으로 set이 가능하다.
- 물론 apllication.set---으로 바로 추가 정의도 가능하다!  
    ![](https://velog.velcdn.com/images%2Fqlgks1%2Fpost%2F71c8c85e-ab86-484d-be28-ffd36ef93021%2Fimage.png)

### 3. out 객체

- out.print와 같이 html 태그를 바로 출력가능하다.

### 4. exception 객체

- 에러 발생시 처리는 jsp파일 상단에 `<%@ page errorPage = "targetError.jsp" %>`와 같은 page 지시어를 추가해놓아야 한다. 

- 특정 페이지를 에러 전용 페이지로 사용하겠다면 `<%@ page isErrorPage = "true" %>` 로 설정해준다.

- 그 에러 페이지 jsp에서 어떤 에러인지 접근하기 위해 exception [[객체(Object)]]에 접근 가능하다!

![](https://velog.velcdn.com/images%2Fqlgks1%2Fpost%2F0abb4ce5-45bc-46ac-851c-39bdbb494f5c%2Fimage.png)

## 데이터 공유

- [[내장 객체]]를 응용해서 servlet에서 데이터를 공유하는 방법은 다음과 같다.
### servlet parameter

- config 객체를 jsp 페이지 내부 뿐 아니라, web.xml에서 정의한 뒤 servlet class에서도 사용이 가능하다.
    - 위 예시 그대로 servlet class내부에서 `String adminId = getServletConfig().getInitParameter("key");` 와 같이 사용하면 된다.
- application 객체, context parameter 또한 똑같이 사용가능하다.