- passport는 여권이라는 이름과 같이 [[서버(Server)]]에서 사용자를 인증하기 위해 사용하는 [[노드(Node.js)]]용 [[미들웨어(Middleware)]]이다.

- 기본적인 인증 시스템 지원과 더불어 구글, 트위터, 페이스북 과 같이 소셜로그인도 passport를 이용해 간편하게 할 수 있다.
- passport는 어플마다 고유의 인증방식이 있음을 간주하고. 각 인증방식을 stragies라는 개별적인 [[모듈(Module)]]로 만든다.
- stragies를 통해 local이나 google과 같은 인증방식을 구현할 수 있다.
- [[NestJS]]는 이러한 토큰 인증(검증)에 있어서 passport의 사용을 권장하고 있다.


## Strategies

- passport.js에서 다양한 인증 방법을 stragies로 제공한다. 
- 공식문서를 보면 다양한 stragies가 존재함을 볼 수 있다. 


