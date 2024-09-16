- [[ExecutionContext]] 인터페이스([[interface]])는 ArgumentsHost 인터페이스([[interface]])를 상위로 두고 있는 것을 확인할 수 있다.

- 즉, context는 [[ExecutionContext]]에서 제공하는 [[메서드(Method)]]와 ArgumentsHost에서 제공하는 (위 ArgumentsHost [[interface]] 참조) [[메서드(Method)]]를 사용할 수 있게 된다.


## ArgumentsHost 역할

- ArgumentsHost는 단순히 [[핸들러(Handler)]]의 인자에 대한 추상화 역할을 한다.
- 예를 들어, [[HTTP(Hyper Tranfer Protocol)]] [[서버(Server)]] 애플리케이션의 경우 (@nestjs/platform-express가 사용되는 경우) 호스트 [[객체(Object)]]는 [[Express]]의 `[request, response, next]` [[배열(Array)]]을 캡슐화한다.

- 여기서 `request`는 요청 객체([[req]]), `response`는 응답 객체([[res]]), [[next()]]는 애플리케이션의 요청-응답 주기를 제어하는 기능을 맡는다. 
- 반면에, [[GraphQL]] 애플리케이션의 경우 호스트 [[객체(Object)]]에는 `[root, args, context, info]` [[배열(Array)]]이 포함된다.


## ArgumentsHost 인터페이스의 구성

- ArgumentsHost 인터페이스([[interface]]) 구성은 아래와 같다.

```ts
export interface ArgumentsHost {
	getArgs<T extends Array<any> = any[]>(): T;
	getArgByIndex<T = any>(index: number): T;
	switchToRpc(): RpcArgumentsHost;
	switchToHttp(): HttpArgumentsHost;
	switchToWs(): WsArgumentsHost;
}
```

- 이처럼 ArgumentsHost는 유용한 메서드 세트를 제공하는 인자 [[배열(Array)]]에 지나지 않는다. 
- 우리가 [[HTTP(Hyper Tranfer Protocol)]] 서버에서 많이 사용하는 [[switchToHttp()]] [[메서드(Method)]] 또한 여기에 포함되어 있는 것을 확인할 수 있다.

