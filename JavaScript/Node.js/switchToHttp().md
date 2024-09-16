- [[ArgumentsHost]]를 확장하여 [[ExecutionContext]] 또한 현재 실행 과정에 추가 정보를 제공하는 새로운 헬퍼들을 추가할 수 있다.

- [[HTTP(Hyper Tranfer Protocol)]]를 사용하고 있다면 switchToHttp() [[메서드(Method)]] 사용하여 요청을 판단하고 [[validateRequest]] 함수가 true 또는 false를 리턴한다.

-  [[NestJS]]의 [[가드(Guard)]]나 [[커스텀 데코레이터(Custom Decorator)]] 등을 구현할 때에는context.switchToHttp().getRequest()를 통해 request를 전달받고 전달받은 요청이 타당한 요청인지 확인하는 작업을 validateRequest()를 통해 진행하면 된다.