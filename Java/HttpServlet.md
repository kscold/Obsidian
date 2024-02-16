- 서블릿([[Servlet]])을 만들기 위해 반드시 [[상속(Inheritance)]]해야 할 필수 [[클래스(Class)]]이다.
- [[HttpServletRequest]]와 [[HttpServletResponse]]를 가지고 있다.
## 메서드
### init()

- 서블릿의 [[객체(Object)]]가 생성 될 때 호출되는 [[메서드(Method)]]이다.
### destroy()

- 서블릿의 객체가 메모리에서 사라질 때 호출되는 메서드이다.
### service()

- 서블릿의 요청이 있을 때 호출되는 메서드이다.
### doGet()

- html에서 form의 메서드가 GET일때 호출되는 메서드이다.
### doPost()

- html에서 form의 메서드가 POST일때 호출되는 메서드이다. 
