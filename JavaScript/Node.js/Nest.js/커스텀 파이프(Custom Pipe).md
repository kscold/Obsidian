- 사용자 정의 파이프는 다양한 용도로 사용될 수 있다.

- 예를들어, 입력 데이터의 유효성검사, 데이터 변환, 비즈니스 로직 처리 등이 가능하다. 
- 이에 따라 [[파이프(Pipe)]] 로직도 다양하게 작성될수도 있다.

- 사용자 정의 파이프를 만드려면 [[PipeTransform]] 인터페이스([[interface]])를 구현하는 [[클래스(class)]]를 작성해야 한다. 
- [[PipeTransform]] 인터페이스([[interface]])는 [[transform()]] [[메서드(Method)]]드를 정의한다. 

- 이 [[메서드(Method)]]는 [[Node.js/Nest.js/NestJS]]가 인자를 처리하고 [[파이프(Pipe)]]가 변환하거나 검증할 값을 입력으로 받고 변환된 값을 반환하기 위해 사용된다.