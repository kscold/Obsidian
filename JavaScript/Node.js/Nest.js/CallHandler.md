- intercept() [[메서드(Method)]]의 두번째 [[매개변수(parameter)]] 형식이다.

- CallHandler 인터페이스([[interface]])는 handle() 메서드를 구현하며, [[인터셉터(Interceptor)]]의 특정 지점에서 라우트 [[핸들러(Handler)]] [[메서드(Method)]]를 호출하는 데 사용할 수 있다.

- intercept() [[메서드(Method)]]의 구현에서 handle() 메서드를 호출하지 않으면 라우트 [[핸들러(Handler)]] [[메서드(Method)]]가 전혀 실행되지 않는다.
  