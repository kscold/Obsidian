- CanActivate는 [[가드(Guard)]]에서 [[상속(Inheritance)]]하는 인터페이스([[interface]])이다.


## CanActivate 구조

- CanActive 인터페이스([[interface]])의 구조는 아래와 같다.

```ts
export interface CanActivate {
    canActivate(context: ExecutionContext): boolean | Promise<boolean> | Observable<boolean>;
}
```

- canActivate()라는 [[메서드(Method)]]가 있으며 [[매개변수(parameter)]]로 [[실행 컨텍스트(execution context)]]([[ExecutionContext]])를 받고 있다.