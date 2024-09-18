- [[서버(Server)]]를 운영하다 보면 [[에러(error)]]가 날 때가 많은데, 이 때마다 어디 부분이 문제인지 정확하게 파악하기 위해서는 어떤 곳에서 [[에러(error)]]가 나는지 로그를 보는 것이 중요하다.
- 따라서 [[NestJS]]에서는 built-in으로 Logger() [[객체(Object)]]를 제공한다.


## 로그의 종류

### Log

- 중요한 정보의 범용 로깅
### Warning 

- 치명적이거나 파괴적이지 않은 처리되지 않은 문제
### Error

- 치명적이거나 파괴적인 처리되지 않은 문제
### Debug

- 개발자 용이다.
- 오류 발생시 로직을 디버그하는 데 도움이되는 유용한 정보이다.
### Verbose

- 운영자 용이다.
- 응용 프로그램의 동작에 대한 통찰력을 제공하는 정보이다.


## 문법

```ts
const logger = new Logger("context"); // 일반적인 Log 용도

private logger = new Logger('name'); // Verbose 용도 name에서 사용중인 것을 명시
```

- "context"을 입력하면 추적하고 싶은 요청만 추적이 가능하다.