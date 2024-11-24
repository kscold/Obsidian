- [[NestJS]]의 [[가드(Guard)]]를 구현하는 과정에서 Execution context([[실행 컨텍스트(execution context)]])를 통해 특정 라우터에 원하는 [[가드(Guard)]]를 적용시켜줄 수 있다.

- 자바스크립트의 [[실행 컨텍스트(execution context)]]와 [[NestJS]]에서 사용한 실행 컨텍스트는 이와는 별개이다.
- 컨텍스트라는 것은 현재 동작의 배경이라는 범용적 개념이다.  

- [[NestJS]]에서 [[실행 컨텍스트(execution context)]]는  특히 가드, [[필터(Filter)]] 등을 작성하는데 사용하게 된다.



## [[NestJS]]의 [[실행 컨텍스트(execution context)]]

- Nest는 여러 애플리케이션 컨텍스트 (예: Nest HTTP server-based, microservices 및 WebSockets 애플리케이션 컨텍스트)에서 작동하는 애플리케이션을 쉽게 작성할 수 있도록 도와주는 몇 가지 유틸리티 [[클래스(class)]]를 제공한다.
- 이러한 유틸리티 [[클래스(class)]]는 광범위한 [[컨트롤러(Controller)]], [[메서드(Method)]] 및 [[실행 컨텍스트(execution context)]]에서 작동할 수 있는 [[제네릭(Generic)]] [[가드(Guard)]], [[필터(Filter)]] 및 [[인터셉터(Interceptor)]]를 구축하는데 사용할 수 있는 현재 실행 컨텍스트에 대한 정보를 제공한다.


## ExecutionContext 인터페이스의 구조

- 인터페이스([[interface]])인 [[ArgumentsHost]]를 인터페이스([[interface]]) ExecutionContext로 [[상속(Inheritance)]]하고 있는 구조이다.

```ts
export interface ExecutionContext extends ArgumentsHost {
    /**
     * Returns the *type* of the controller class which the current handler belongs to.
     */
    getClass<T = any>(): Type<T>;
    /**
     * Returns a reference to the handler (method) that will be invoked next in the
     * request pipeline.
     */
    getHandler(): Function;
}
```

- 즉, context는 ExecutionContext에서 제공하는 메서드와 [[ArgumentsHost]]에서 제공하는 [[메서드(Method)]]를 사용할 수 있게 된다.
