- 대기열에 비유할 수 있다.
- 먼저 들어간 데이터가 먼저 나가는 선입선출(First In First Out) 구조를 가지고 있다.

```python
from collections import deque

# 큐(Queue) 구현을 위해 deque 라이브러리 사용
queue = deque()

queue.append(5)
queue.append(2)
queue.append(3)
queue.append(7)
queue.popleft()
queue.append(1)
queue.append(4)
queue.popleft()

print(queue) # 먼저 들어온 순서대로 출력
queue.reverse() # 다음 출력을 위해 역순으로 바꾸기
print(queue) # 나중에 들어온 원소부터 출력

>> dequeue([3, 7, 1, 4])
>> dequeue([4, 7, 1, 4])
```

- 파이썬으로 큐를 구현할 때는 collections 모듈에서 제공하는 [[deque()]] 자료구조를 활용한다.
