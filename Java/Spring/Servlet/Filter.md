- 필터를 구현하기 위해 [[Servlet]]/[[JSP]]에서는 javax.servlet.Filter라는 [[인터페이스(Interface)]]를 제공하며 이 인터페이스를 구현하도록 되어 있다.


## Filter 메서드

- Filter 인터페이스의 [[메서드(Method)]]는 다음과 같이 구성되어있다.

```java
package javax.servlet;

import java.io.IOException;

public interface Filter {
	public void init(FilterConfig filterConfig) throws ServletException;
	
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException;
	
	public void destroy();	
}
```
  

public void init(FilterConfig filterConfig)

웹컨테이너(톰캣)이 시작될 때 필터 객체를 생성하는데, 이때 객체가 생성되면서 최초에 한 번 호출되는 메서드입니다. FilterConfig 객체를 넘겨주기 때문에 이를 통해 여러가지 설정값을 넘겨받을 수 있고 필터에서 처리시 필요한 객체등을 초기화(예를들어 JDBC 커넥션 등) 하는데 사용됩니다. 

  

public void destroy()

필터 객체가 제거될 때 실행되는 메서드입니다. 보통 초기화시 생성했던 자원들을 종료하는 기능에 사용됩니다.

public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) 

필터의 핵심 메서드입니다. doFilter()는 클라이언트의 요청이 있을때마다 매번 실행됩니다. ServletRequest와 ServletResponse 객체를 넘겨주기 때문에 이를 가지고 요청과 응답을 조작할 수 있습니다. 그리고 FilterChain을 통해 조작 이후 요청을 원래 목적지인 서블릿으로 전달할 수 있습니다. 

출처: [https://dololak.tistory.com/598](https://dololak.tistory.com/598) [코끼리를 냉장고에 넣는 방법:티스토리]