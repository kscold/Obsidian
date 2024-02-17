- 웹 어플리케이션 서버(WAS)는 [[HTTP]] 기반으로 동작하며, [[웹 서버(Web Server)]]의 기능도 포함하기 때문에 정적 리소스를 제공할 수 있다. 
- 
- 사용자의 요청이 들어오면 프로그램 코드를 실행해서 애플리케이션 로직을 수행한 후, 그 결과를 HTML이나 HTTP API([[JSON(Java Script Object Notation)]])으로 제공한다.  

- WAS는 서블릿([[Servlet]]) 컨테이너를 포함하는 개념이다.(거의 동일하다.)

- 매 요청마다 쓰레드([[Thread]]) 풀에서 기존 쓰레드를 사용한다.
- 주요 튜닝 포인트는 max thread 수이다.

- 웹 컨테이너라고도 부른다.

- 예시로 [[톰캣(tomcat)]], Jetty, Undertow가 있다.


## 웹 서버와 웹 어플리케이션의 차이

- 간단하게 살펴보면 웹 서버는 정적 리소스(파일)를 제공하고, WAS는 어플리케이션 로직까지 실행할 수 있다는 점이다.

- 하지만 웹 서버도 프로그램을 실행하는 기능을 포함하기도하고, WAS 역시 웹 서버의 기능을 제공하기 때문에 둘은 용어도 경계도 모호하다.

- 허나, 웹 서버와 WAS에 구분을 두는 이유는 WAS에서 정적 리소스까지 서빙한다면 WAS는 너무 많은 역할을 담당하게 된다. 

- 자칫 비용이 비싼 [[비즈니스 로직(Business Logic)]]을 수행함에 있어, 정적 리소스 때문에 어려움을 겪을 수 있다. 또한, WAS는 [[비즈니스 로직(Business Logic)]]을 수행하기 때문에 빈번하게 서버 다운이 발생한다.

- 만약 정적 리소스 서빙까지 맡고 있던 WAS 서버에 장애가 발생하면 오류 화면을 노출시킬 수 없는 상황이 발생한다.
- API로 데이터만 전달하는 경우에는 상관 없다.

![](https://blog.kakaocdn.net/dn/cC1SbE/btrKIyqWPto/H3TzSzl5fIwO0GwRXphBvk/img.png)

- 바로 이런 문제를 해결하고자, WAS 앞단에 웹 서버를 두어 정적 리소스는 웹서버가 처리하고, 어플리케이션 로직같은 동적인 처리가 필요하면 WAS에게 위임하여 처리하도록 한다. 
- 이렇게 하면 WAS는 중요한 비지니스 로직만 처리하면 되기 때문에 부담이 줄어든다.

![](https://blog.kakaocdn.net/dn/bBo2aj/btrKGQ0pr3A/CCHcNxj9f0dbCCHIzBB380/img.png)

- 리소스 관리도 효울적으로 할 수 있다. 
- 정적 리소스가 많이 사용되면 웹 서버를 증설하고, 어플리케이션 리소스가 많이 사용되면 WAS를 증설하면 된다.

![](https://blog.kakaocdn.net/dn/cWJMDB/btrKHvOYrY2/V2PdVv6S4rKXqPtgOsvuSK/img.png)
