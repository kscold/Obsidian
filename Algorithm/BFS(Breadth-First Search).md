v- BFS는 너비 우선 탐색이라고 부르며, 그래프에서 가까운 노드부터 우선적으로 탐색하는 알고리즘이다.
- BFS는 [[큐(Queue)]] 자료구조를 이용한다.
- [[시간 복잡도]]는 O(n)이 걸린다.
- [[DFS(Depth-First Search)]]보다 조금 더 빠르다.
- BFS는 자동적으로 최단경로를 찾는다.



## BFS 알고리즘

1. 탐색 시작 노드를 큐에 삽입하고 방문 처리를 한다.
2. 큐에서 노드를 꺼낸 뒤에 해당 노드의 인접 노드 중에서 방문하지 않은 노드를 모두 큐에 삽입하고 방문처리를 한다.
3. 더 이상 2번의 과정을 수행할 수 없을 때까지 반복한다.

- 밑에 코드는 BFS를 구현하는 예시이다.

```python
from collections import deque  
  
  
# BFS 메서드 정의  
def bfs(graph, start, visited):  
    # 큐(Queue) 구현을 위해 deque 라이브러리 사용  
    queue = deque([start]) # 시작 노드를 큐에 넣고 시작  
    # 현재 노드를 방문 처리  
    visited[start] = True  
    # 큐가 빌 때까지 반복  
    while queue:  
        # 큐에서 하나의 원소를 뽑아 출력  
        v = queue.popleft()  
        print(v, end=' ')  
        # 해당 원소와 연결된, 아직 방문하지 않은 원소들을 큐에 삽입  
        for i in graph[v]:  
            if not visited[i]:  
                queue.append(i) # 인접 리스트의 노드를 큐에 추가하고  
                visited[i] = True # 인접 리스트의 노드도 방문 처리, 이 부분이 DFS와 큰 차이
  
  
# 각 노드가 연결된 정보를 리스트 자료형으로 표현(2차원 리스트)  
graph = [  
    [],  
    [2, 3, 8],  
    [1, 7],  
    [1, 4, 5],  
    [3, 5],  
    [3, 4],  
    [7],  
    [2, 6, 8],  
    [1, 7]  
]  
  
# 각 노드가 방문된 정보를 리스트 자료형으로 표현(1차원 리스트)  
visited = [False] * 9  
  
# 정의된 BFS 함수 호출  
bfs(graph, 1, visited)
```


## 미로 찾기 알고리즘
- BFS는 주로 미로 찾기 알고리즘에 많이 쓰이며, 최단 경로를 반환하기 때문에 움직이는 횟수를 저장하는 구현으로 만들어나간다.

```python
from collections import deque  
  
n, m = map(int, input().split())  
  
gragh = []  
for i in range(n):  
    gragh.append(list(map(int, input())))  
  
dx = [-1, 1, 0, 0]  
dy = [0, 0, -1, 1]  
# 상, 하, 좌, 우  
  
  
def bfs(x, y):  
    queue = deque()  
    queue.append((x, y)) # 시작 좌표를 큐에 넣고 시작  
    # 큐가 빌 때까지 반복  
    while queue:  
        x, y = queue.popleft()  
        # 현재 위치에서 네 방향으로의 위치 확인  
        for i in range(4):  
            nx = x + dx[i]  
            ny = y + dy[i]  
            # 미로 찾기 공간을 벗어날 경우 무시  
            if nx < 0 or ny < 0 or nx >= n or ny >= m:  
                continue  
            # 벽(괴물)인 경우 무시  
            if gragh[nx][ny] == 0:  
                continue  
            # 해당 노드를 처음 방문하는 경우에만 최단 거리 기록  
            if gragh[nx][ny] == 1:  
                gragh[nx][ny] = gragh[x][y] + 1 
                # 그 좌표에 현재까지의 거리를 저장, 방문을 저장하는 것이 아니라 값을 저장하는 것임  
                queue.append((nx, ny)) # 인전 노드 좌표를 큐에 넣음
    
    # 가장 오른쪽 아래까지의 최단 거리 반환  
    return gragh[n - 1][m - 1] # 가장 마지막칸에 최단경로의 횟수가 저장되어 있음  
  
  
# BFS를 수행한 결과 출력  
print(bfs(0, 0))
```