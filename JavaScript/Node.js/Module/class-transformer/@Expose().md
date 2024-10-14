- @Exclude() [[데코레이터(Decorator)]]는 [[class-transformer]]에서 제공하는 [[데코레이터(Decorator)]] 중 하나로, [[클래스(class)]]의 특정 [[속성(Property)]]을 [[직렬화(Serialization)]] 또는 [[역직렬화 (Deserialization)]] 시 제외하고 싶을 때 사용된다.

- 즉, [[클래스(class)]] [[인스턴스(Instance)]]를 평범한 자바스크립트 [[객체(Object)]]로 변환하거나 [[JSON(Java Script Object Notation)]]으로 [[직렬화(Serialization)]]할 때, 해당 [[속성(Property)]]을 포함하지 않도록 설정할 수 있다.

- 예를 들어, API 응답에서 비밀번호와 같은 민감한 데이터를 제외하고 싶을 때 사용할 수 있다.
- API 응답에서는 필요하지 않은, 내부적으로만 사용되는 값을 제외할 수 있다.