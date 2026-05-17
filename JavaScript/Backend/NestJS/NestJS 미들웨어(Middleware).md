- [[NestJS]]에서는 [[파이프(Pipe)]], [[필터(Filter)]], [[가드(Guard)]], [[인터셉터(Interceptor)]] 등의 [[미들웨어(Middleware)]]로 취급되는 것들이 있는데 각각 다른 목적을 가지며 사용되고 있다.

## NestJS 미들웨어의 종류
### [[파이프(Pipe)]]

- [[파이프(Pipe)]]는 요청 유효성 검사 및 페이로드 변환을 위해 만들어진다.
- 데이터를 예상한 대로 직렬화 한다.
### [[필터(Filter)]]

- [[필터(Filter)]]는 [[에러 미들웨어(Error Middleware)]]이다.
- 특정 오류 처리기를 사용할 경로와 각 경로 주변의 복잡성을 관리하는 방법을 알 수 있다.
### [[가드(Guard)]]

- [[가드(Guard)]]는 인증 [[미들웨어(Middleware)]]이다.
- 지정된 경로로 통과할 수 있는 사람과 허용되지 않는 사람을 서버에 알려준다.
### [[인터셉터(Interceptor)]]

- [[인터셉터(Interceptor)]]는 응답 매핑 및 캐시 관리와 함께 요청 로깅과 같은 전후 [[미들웨어(Middleware)]]이다.
- 각 요청 전후에 이를 실행하는 기능은 매우 강력하고유용하다.



## 각각 [[미들웨어(Middleware)]]가 불러지는(called) 순서

- 아래 흐름은 각가의 [[미들웨어(Middleware)]]가 움직이는 순서이다.
- 즉, [[NestJS]]의 [[미들웨어(Middleware)]]들은 [[NestJS 생명 주기(LifeCycle)]]을 가지고 있다.

![[Pasted image 20241124170503.png]]

- [[미들웨어(Middleware)]] -> [[가드(Guard)]] -> [[인터셉터(Interceptor)]](before) -> [[파이프(Pipe)]] -> [[컨트롤러(Controller)]] -> [[서비스(Service)]] -> [[인터셉터(Interceptor)]](after) -> [[필터(Filter)]] (if applicable) -> 클라이언트(client)

![[Pasted image 20241124170436.png]]