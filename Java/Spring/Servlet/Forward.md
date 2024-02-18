- 서블릿([[Servlet]])에게 클라이언트(웹 브라우저)를 거치지 않고 바로 다른 서블릿(또는 [[JSP]])에게 요청하는 방식이다.
- Forward 방식은 서버 내부에서 일어나는 요청이기 때문에 [[HttpServletRequest]], [[HttpServletResponse]] [[객체(Object)]]가 새롭게 생성되지 않는다.


```java
RequestDispatcher dispatcher = request.getRequestDispatcher("포워드 할 서블릿 또는 JSP")

dispatcher.forward(request, response)
```