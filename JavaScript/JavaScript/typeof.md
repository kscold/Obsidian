- [[원시 타입(Primitive type)]]이나 [[참조 타입(Reference Type)]]이 어떤 타입을 가지고 있는지 확인할 수 있는 [[키워드(Keyword)]]이다.

- [[참조 타입(Reference Type)]]에서 [[배열(Array)]]의 경우, 내부적으로 [[배열(Array)]]도 [[객체(Object)]]이기 때문에 Object라고 뜨는데 제대로 확인을 위해서는 typeof대신 Array [[클래스(Class)]]에 정의 되어 있는 정적 메서드인 Array.isArray()를 사용해서 확인하면 된다.