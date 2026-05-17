- 서블릿([[Servlet]]) 컨테이너가 서블릿을 생성하고 초기화할 때 전달하는 설정에 대한 [[인터페이스(Interface)]]이다.

## [[메서드(Method)]]

```java
package javax.servlet;


public interface ServletConfig {
    String getServletName();
    // 서블릿의 이름을 반환한다
    // 이 이름은 서버관리자가 지정해 주거나, 웹 애플리케이션에서 배포 설명자 혹은 클래스이름으로 정해지게 된다
    
    ServletContext getServletContext();
    // 실행중인 caller가 참조하는 ServletContext를 반환한다
    // ServletContext에 대해서는 항목 3 에서 정리한다
    
    String getInitParameter(String name);
    // 초기화에 사용될 파라미터 이름`name`에 해당하는 값을 반환한다
    // 해당 초기화 파라미터가 없다면 null 을 반환한다
    
    Enumeration<String> getInitParameterNames();
    // 초기화 할 파라미터들의 이름에 대한 Enumeration을 가져온다
    // 이름은 String형이므로 반환형은 Enumeration<String>이다
}
```