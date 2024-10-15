- [[NestJS]]는 애플리케이션이 부트스트랩 되어, 프로세스가 종료될 때 까지의 라이프사이클을 가지고 있다.

## LifeCycle 다이어그램

![notion image](https://www.rldnd.net/_next/image?url=https%3A%2F%2Fwww.notion.so%2Fimage%2Fhttps%253A%252F%252Fs3-us-west-2.amazonaws.com%252Fsecure.notion-static.com%252F442cd1be-1041-4a55-b613-f7fe3a999b63%252Flifecycle-events.png%3Ftable%3Dblock%26id%3D07cd8b96-ffc5-4650-8a62-6dcb08174c22%26cache%3Dv2&w=2048&q=75)

- 순서는 아래와 같이 간단하게 3가지로 정리할 수 있다.

1. 초기화
2. 실행
3. 종료

| 수명 주기 후크 방법                    | 후크 메서드 호출을 트리거하는 수명 주기 이벤트                                                                                         |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------ |
| `onModuleInit()`               | 호스트 모듈의 종속성이 해결되면 호출                                                                                               |
| `onApplicationBootstrap()`     | 모든 모듈이 초기화된 후 연결을 수신 대기하기 전에 호출                                                                                    |
| `onModuleDestroy()`*           | 종료 신호(예: `SIGTERM`)가 수신된 후 호출                                                                                      |
| `beforeApplicationShutdown()`* | 모든 `onModuleDestroy()`처리기가 완료된 후 호출 ( Promise 해결 또는 거부 ) 완료되면(Promise가 해결되거나 거부됨) 모든 기존 연결이 닫힘 ( `app.close()`호출 ) |
| `onApplicationShutdown()`*     | 연결 종료 후 호출 ( `app.close()`해결 )                                                                                     |

- * 로 표시한 후크의 경우 명시적으로 호출하지 않는다면, enableShutdownHooks() 를 호출해주어야 한다.
- 이 수명 주기를 사용하여 모듈 및 서비스의 적절한 초기화를 계획하고, 활성 연결을 관리하고, 종료 신호를 받으면 애플리케이션을 정상적으로 종료할 수 있다.


## 사용방법

- 인터페이스([[Interface]])를 이용한다.
- 인터페이스 같은 경우 당연하게도 [[타입스크립트(TypeScript)]] 문법이기에, 컴파일이 되면 사라지게 된다.

- 그러므로 선택 사항이지만 당연히도 쓰는게 좋을 것이다.

- 당연히도 [[async await]] 표현을 통해 [[비동기(asynchronous)]]적으로 초기화 또한 가능하다.

### onModuleInit()

```ts
import { Injectable, OnModuleInit } from '@nestjs/common';

@Injectable() export class UsersService implements OnModuleInit {    // sync  
	onModuleInit() { 
		console.log(`The module has been initialized.`);  
	}    
	
	// async   
	async onMoudleInit(): Promise<void> {     
		await this.fetch();   
	}    
	
	async fetch(): Promise<void> {     
		await db.getData(); // example   
	} 
}
```


### 앱 종료

- `onModuleDestroy()`, `beforeApplicationShutdown()` 의 경우 종료 단계에서 호출된다.

- `onApplicationShutdown` 의 경우 시스템 신호에 대해 응답을 하게 되는데, 보통 k8s 와 함께 사용된다고 한다.

- 종료 후크를 사용하려면 그에 대한 [[이벤트 리스너(Event Listener)]]를 활성화 해야하는데, 이는 `enableShutdownHooks()` 를 호출하면 된다.

- `bootstrap()` 시 내부에서 shutdown에 대한 리스너를 달고 있다고 생각하면 되겠다.

```ts
import { NestFactory } from '@nestjs/core'; 
import { AppModule } from './app.module';  

async function bootstrap() {   
	const app = await NestFactory.create(AppModule);    // shutdown hooks에 대한 리스닝. 이 후크를 무조건 먼저 활성화 해야함   
	app.enableShutdownHooks();  	
	
	await app.listen(3000); 
}  

bootstrap();
```


- 종료하라는 신호를 받게 되면, `onModuleDestroy()`, `beforeApplicationShutdown()`, `onApplicationShutdown()` 메서드를 호출하게 되는데, 첫 번째 매개 변수로 해당 신호를 받는다.

- 등록된 함수들이 async인 경우, Nest는 resolve / reject 될 때까지 기다리게 된다.

```ts
@Injectable() class UsersService implements OnApplicationShutdown {  
	onApplicationShutdown(signal: string) {     
		console.log(signal); // e.g. "SIGINT"   
	} 
}
```

## 로직 수행의 흐름

![notion image](https://www.rldnd.net/_next/image?url=https%3A%2F%2Fwww.notion.so%2Fimage%2Fhttps%253A%252F%252Fs3-us-west-2.amazonaws.com%252Fsecure.notion-static.com%252F5ab40eb5-d954-4b6a-b2ff-395f0a602301%252Fimages_haron_post_e2587453-9aa2-4f2d-9ae4-0c8c024ed42f_image.png%3Ftable%3Dblock%26id%3D932e63d8-011c-4398-851e-823db875cbe7%26cache%3Dv2&w=3840&q=75)

### 1. Request
### 2. [[미들웨어(Middleware)]] 
### 3. [[가드(Guard)]]

- 주로 permission ( 인증 ) 처리를 할 때 사용한다.
### 4. pre [[인터셉터(Interceptor)]]

- 주로 post-interceptor를 위한 변수 선언, 함수 실행(optional)이다.
### 5. [[파이프(Pipe)]]

- 변환( 요청 바디를 원하는 형식으로 변환 ), 유효성 검사이다.
### 6. [[컨트롤러(Controller)]]

- 라우터 역할을 수행한다.
### 7. [[서비스(Service)]]

- 해당 요청에 대한 핵심 로직이 수행한다.
### 8. post [[인터셉터(Interceptor)]]

- 주로 pre-interceptor 로직을 가지고 응답한 데이터를 가공하거나 전체 로직의 속도 측정한다.
- 최종적으로 성공적인 응답 데이터를 보낸다.
### 9. exception [[필터(Filter)]]

- 예외 처리를 담당한다.
- 에러 메세지를 원하는 형태로 가공해서 응답한다.
### 10. Response