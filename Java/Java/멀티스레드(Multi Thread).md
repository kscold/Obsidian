- Multi Thread는 말 그대로 하나의 Process에서 [[Thread]]를 여러개 사용하는 것을 말한다.
- 이 때, Process 내에 존재하는 [[Thread]]는 메모리의 Heap영역과 Data영역을 공유하게 된다.

- 이 메모리를 공유하는 것은 적절하게 사용한다면 Context Switching에 사용되는 비용을 최소화해 애플리케이션의 성능을 향상시킬 수 있다.

- 하지만, 부적절하게 사용한다면 여러 Thread들이 공유된 특정 자원에 동시에 접근할 경우 문제가 발생할 수 있는데, 이 때 발생할 수 있는 문제점 중 하나가 [[Race Condition]]이다.