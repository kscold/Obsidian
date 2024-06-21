- [[app.use()]]의 next() 메서드는 [[콜백 함수(Callback Function)]]의 [[매개변수(parameter)]]로 다음 작업 혹은 [[미들웨어(Middleware)]]를 실행하는 [[함수(Function)]]이다.

- 만약 next()의 경우 인자가 있으면 그 미들웨어로 이동한다.


## next(error 객체)

- try/catch와 같은 구문으로 catch에 [[에러(error)]] [[객체(Object)]]가 잡혔을 때 이 [[객체(Object)]]를 인자로 next() [[메서드(Method)]]에 넣으면 바로 [[에러 미들웨어(Error Middleware)]]로 이동 시키도록 만들 수 있다.


## next('route')

- next('route')로 정의했을 경우에는 다음 [[콜백 함수(Callback Function)]] 작업이 있다면 이 다음 작업이 실행되는 것이 아니라 건너뛰고 아예 다름 [[미들웨어(Middleware)]]로 실행되게 만든다.

- 주로 조건문을 걸어 다음 [[미들웨어(Middleware)]]로 이동하게 구현한다.