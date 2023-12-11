## 힙 함수 활용하기

- heapq.heappush(heap, item) : item을 heap에 추가
- heapq.heappop(heap) : heap에서 가장 작은 원소를 pop & 리턴. 비어 있는 경우 IndexError가 호출됨. 
- heapq.heapify(x) : 리스트 x를 즉각적으로 heap으로 변환함 (in linear time, O(N))

#### 힙 생성 & 원소 추가

heapq 모듈은 리스트를 최소 힙처럼 다룰 수 있도록 하기 때문에, 빈 리스트를 생성한 후 heapq의 함수를 호출할 때마다 리스트를  인자에 넘겨야 한다.

아래 코드는 heap이라는 빈 리스트를 생성하고 50, 10, 20을 각각 추가하는 예시이다.

```python
import heapq

heap = []
heapq.heappush(heap, 50)
heapq.heappush(heap, 10)
heapq.heappush(heap, 20)

print(heap)

>> [10, 50, 20]
```