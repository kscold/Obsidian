- 서비스는 소프트웨어 개발 내의 공통 개념이며 [[NestJS]]나 자바스크립트에서만 쓰이는 개념은 아니다.
- 서비스는 요청과 응답에 대해서는 모르며, 해야 되는 것만 동작하고 그 결과 값만 다시 [[컨트롤러(Controller)]]로 반환되어야 한다.

- [[@Injectable()]] [[데코레이터(Decorator)]]로 감싸져서 [[NestJS 모듈(module)]]에 제공되며, 이 서비스 [[인스턴스(Instance)]]는 어플리케이션 전체에서 사용될 수 있다.

- 서비스는 [[컨트롤러(Controller)]]에서 데이터의 유요성 체크를 하거나 [[데이터베이스(DataBase)]]에 아이템을 생성하는 등의 작업을 하는 부분을 처리한다.

