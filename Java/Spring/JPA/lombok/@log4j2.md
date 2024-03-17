- 가장 최신에 나온 로깅 프레임워크로써 [[아파치(apache)]]의 log4j의 다음 버전이다.

- logback처럼 필터링 기능과 자동 리로딩을 지원한다.
- logback과의 가장 큰 차이는 [[멀티스레드(Multi Thread)]] 환경에서 [[비동기(Asynchronous)]] 로거(Async Logger)의 경우 다른 [[로깅(logging)]] 프레임워크보다 처리량이 훨씬 많고, 대기 시간이 훨씬 짧다. 

- 또한, 자바 8부터 도입된 [[람다(lambda)]]식을 지원하고, Lazy Evalutaion을 지원한다.

- 따라서 [[롬복(lombok)]]의 소속된 @log4j2 [[어노테이션(Annotation)]]을 붙이면 [[로깅(logging)]]을 할 수 있다.