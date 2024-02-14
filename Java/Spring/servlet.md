- servlet(서블릿)은 클라이언트의 요청을 처리하고, 그 결과를 반환하는 Servlet [[클래스(Class)]]의 구현 규칙을 지킨 자바 웹 프로그래밍 기술이다.

- Server + Applet(플러그인의 하나 작은 프로그램을 의미)의 합성어이다.
- 즉, 동적 웹페이지 서버에서 수행되는 소형 프로그램이고 구현하기 위한 표준이다.

- 자바를 웹어플리케이션에 조금 더 개발하기 쉽게 하기 위해 만든 API(라이브러리, 클래스 들)이며 이 규약에 맞는 라이브러리나 [[클래스(Class)]]들을 [[상속(Inheritance)]] 및 구현하여 만든 클래스들을 서블릿이라고 한다.

![[Pasted image 20231101230754.png]]

- 서버사이드에서 돌아가는 java program이다.
- 서블릿(servlet)은 자바를 사용해 웹 페이지를 동적으로 생성하는 서버내 프로그램이라고 한다.

- 각 사용자의 요청이 서버의 하나의 쓰레드([[Thread]])로 수행된다. 

- [[JSP]]와 달리 자바코드 안에 html을 포함하고 있다는 점이 다르다.(JSP는 html에 자바 코드 포함  .java 확장자)

- data processing에 좋다.([[컨트롤러(Controller)]])

## 실제 서블릿의 다이어그램

![[Pasted image 20231101230803.png]]

- 사용자는 실제로 ServletEx와 같은 Class만 만들면 된다.

## ServletContainer

- 서블릿의 생성부터 소멸까지의 라이프사이클을 관리하는 역할을 한다.([[스프링 컨테이너(Spring Container)]]의 개념과 비슷하다.)
- 서블릿 컨테이너는 웹 서버와 소켓을 만들고 통신하는 과정을 대신 처리해준다.
- 즉, 개발자는 [[비즈니스 로직(Business Logic)]]만 집중하면 된다.
- 서블릿 [[객체(Object)]]를 [[싱글톤(Singleton)]]으로 관리한다. ([[인스턴스(Instance)]] 하나만 생성하여 공유하는 방식이다.)
	- 상태를 유지(stateful)하게 설계하면 안된다.
	- Thread safety 하지 않는다.

## 서블릿의 [[MVC(Mode View Controller)]] 패턴

- MVC 패턴에서 [[컨트롤러(Controller)]]로 이용한다.

![[Pasted image 20231101230919.png]]

1. 사용자(클라이언트)가 URL을 입력하면 HTTP Request가 Servlet Container로 전송한다.
2. 요청을 전송받은 Servlet Container는 HttpServletRequest, HttpServletResponse [[객체(Object)]]를 생성한다.
3. web.xml을 기반으로 사용자가 요청한 URL이 어느 서블릿에 대한 요청인지 찾는다.
4. 해당 서블릿에서 service [[메서드(Method)]]를 호출한 후 클리아언트의 GET, POST여부에 따라 doGet() 또는 doPost()를 호출한다.
5. doGet() or doPost() 메서드는 동적 페이지를 생성한 후 HttpServletResponse객체에 응답을 보낸다.
6. 응답이 끝나면 HttpServletRequest, HttpServletResponse 두 객체를 소멸시킨다.

```java
package com.servlet.test;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class HelloServlet
 */
@WebServlet("/hs") // 어노테이션으로 서블릿 맵핑 한 것 (url) -> 대신 web.xml에서 정의해줄 수 있음
public class HelloServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public HelloServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

		PrintWriter out = response.getWriter();
		out.print("<html>");
		out.print("<head>");
		out.print("</head>");
		out.print("<body>");
		out.print("<div>");
		out.print("<h1> HELLO SERVLET </h1>");
		out.print("</div>");
		out.print("</body>");
		out.print("</html>");
		
		// TODO Auto-generated method stub
		response.getWriter().append("Served at: ").append(request.getContextPath());
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		doGet(request, response);
	}

}
```

### HttpServletRequest

- 요청에 대한 정보를 가지고 있는 객체이다.
	- request.getCookies();
	- request.getSession();
	- request.getAttribute(null);
	- request.setAttribute(null, null);
	- request.getParameter(null);
	- request.getParameterNames();
	- request.getParameterValues(null);

### HttpServletResponse

- 응답에 대한 정보를 가지고 있는 객체이다.
	- response.addCookie(null);
	- response.getStatus();
	- response.sendRedirect(null);
	- response.getWriter();
	- response.getOutputStream();


## Life Cycle(생명주기)

- 생성(init) 단계에서 body(request 패킷에서 data에 대해)등의 자원 관련 작업을 많이 한다.
- service에서 http 요청(requset)에 해당하는 메서드가 실행된다.

![[Pasted image 20231101232203.png]]

![[Pasted image 20231101232216.png]]

