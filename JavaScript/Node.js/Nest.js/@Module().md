- @Module() [[데코레이터(Decorator)]]는 단일 객체를 인자로 받으며, 인자로서 사용되는 단일 객체는 [[NestJS 모듈(module)]]의 형태를 나타내는 속성들로 구성되어있다.


## @Module()의 속성

## providers

- 해당 모듈 이외에 공유되는 [[providers]]들을 정의하거나, 또는 [[의존성 주입(Dependency Injection)]]을 위해 [[인스턴스(Instance)]]화 될 [[providers]]들을 정의하기 위한 속성이다.

## controllers

- 해당 [[NestJS 모듈(module)]] 내에서 정의된 [[인스턴스(Instance)]]화 되어야 할 [[컨트롤러(Controller)]]들의 집합을 정의하기 위한 속성이다.

## imports

- 해당 [[NestJS 모듈(module)]]에서 필요한 외부 [[providers]]들을 Export 하는 외부 [[NestJS 모듈(module)]]들의 집합을 정의하기 위한 속성이다.

## exports

- 해당 모듈로부터 제공되는 여러 [[providers]] 들을 정의하기 위한 속성이다.
- 해당 모듈을 Import 한 다른 모듈들은 해당 모듈에서 제공하는 [[providers]]들을 사용할 수 있다. 
- 외부 모듈들은 해당 속성을 통해 제공되는 [[providers]]만 사용할 수 있다는 것이다.