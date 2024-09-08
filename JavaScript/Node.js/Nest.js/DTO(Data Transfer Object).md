- DTO는 데이터 전송 객체(Data Transfer Object)의 약자로, 클라이언트에서 서버로 전달되는 데이터의 형식을 지정하는 역할을 한다.

- 서버 측에서 요청을 처리하기 전에 데이터의 유효성을 검사하고, 해당 데이터를 가공하고, [[데이터베이스(DataBase)]]와 통신하기 전에 데이터를 변환하는 등의 역할을 수행한다.

- [[NestJS]]에서는 이러한 DTO를 쉽게 작성할 수 있는 [[클래스(class)]] 형태로 제공하며, [[class-validator]]와 같은 라이브러리를 사용하여 데이터 유효성 검사를 지원한다.


## DTO의 사용 이유

- [[NestJS]]에서 [[모델(Model)]]을 정의하기 위한 [[속성(Property)]]들을 여러 곳에서 상용한다.
- 간단한 어플리케이션을 만들 때, [[모델(Model)]]의  [[속성(Property)]]을 수정하게 될 시에 직접 수정을 하면 되지만, 정말 많은 [[모델(Model)]]의  [[속성(Property)]]을 가지고 있다면 유지보수하기 쉽지 않다.

- 이런한 경우에 DTO(Data Transfer Object)를 사용하여 유지보수하기 편하게 만들 수 있다.


## Class vs Interface

- DTO 파일을 작성할 때, [[클래스(class)]]는 [[Interface]]