- 노드의 개수가 n이라면 n X n 형태의 2차원 배열로 그래프의 연결 관계를 표현한다.

- 인접 행렬에서 행과 열은 노드를 의미하며, 해당 위치의 값은 두 노드 사이의 관계(간선의 존재 여부 또는 가중치)를 나타낸다.  
- `adj[i][j]` : 노드 i에서 노드 j로 가는 간선이 있으면 1, 아니면 0으로 표현한다.
- 노드와 관련되어 있는 에지를 탐색하려면 N번 접근해야하므로 노드의 갯수에 비해 공간복잡도 + 시간복잡도가 좋지 않다.(그래도 [[에지 리스트(edge list)]]보다는 좋다.)

[[그래프(Graphs)]]는  가중치가 없는 [[무방향 그래프(Undirected graph)]]와 가중치가 있는 [[유방향 그래프(Digraph(Directed graph)]]로 나눌 수 있다.

가중치가 없는 무방향 그래프
: 연결되어 있는 경우 행렬에서 1의 값을 가지고, 연결되지 않은 경우 0의 값을 가진다. 두 개의 노드에서 간선이 동시에 연결되어 있기 때문에 인접 행렬이 대칭적 구조를 가진다.


가중치가 있는 유방향 그래프는 행렬  
: 행렬에서 각 간선의 유무(0과 1) 대신 가중치의 값이 저장된다. 이 경우 가중치가 0인 것과 간선이 없는 것이 구별돼야 한다.

![[Pasted image 20230919231718.png]]


**장점:** 2차원 배열에 모든 노드들의 간선 정보가 있기 때문에, 두 노드를 연결하는 간선을 조회할 때 O(1) 시간복잡도를 가진다.  
**단점:** 간선의 수와 무관하게 항상 n² 크기의 2차원 배열이 필요하므로 메모리 공간이 낭비된다.  
그래프의 모든 간선의 수를 알아내려면 인접행렬 전체를 확인해야 하므로 O(n²)의 시간이 소요된다.


- 아래는 인접 행렬를 만드는 코드의 예시이다.

- 이코테 예시
```python
INF = 999999999  # 무한의 비용 선언  
  
# 2차원 리스트를 이용해 인접 행렬 표현  
graph = [  
    [0, 7, 5],  # weight, left, right , 인덱스 0 이 data    
    [7, 0, INF],  # 인덱스 1이 data    
    [5, INF, 0]  # 인덱스 2이 data
]
```

```python
V, E = map(int, input().split())  # 정점의 개수와 간선의 갯수를 입력받음  
edge = list(map(int, input().split()))  # 간선의 정보를 입력받음  
adj = [[0] * (V + 1) for _ in range(V + 1)]  # 첫번째 인덱스 0를 1부터 받기 위해서 V x V가 아니라 V + 1 x v + 1 을 행렬을 초기화  
  
for i in range(E):  # 간선의 갯수 만큼 반복  
    n1 = edge[i * 2]  
    # 첫번째 인덱스 0를 건너뛰고 받기 위해서 edge[0] edge[2] edge[4] edge[6] edge[8]    
    n2 = edge[i * 2 + 1] 
    # 첫번째 인덱스 0를 건너뛰고 두번째 인덱스를 받기 위해서 edge[1] edge[3] edge[5] edge[7]
    
    adj[n1][n2] = 1  # 위에서 받은 정보를 토대로 값이 있는 위치에 1로 표기  
    
    adj[n2][n1] = 1  # 무향 그래프인 경우 대칭  
  
print(adj)  # 이 위치로 이동하여 한 번만 출력되도록 수정
```

인풋값

```
>> 4 # V 정점의 갯수 입력
>> 3 # E 간선의 갯수 입력
>> 1 2 2 3 3 4 # 간선의 정보들을 입력
```

위의 인풋값으로 인접행렬을 그리면

```
  0 1 2 3 4 
0 0 0 0 0 0 
1 0 0 1 0 0 
2 0 1 0 1 0 
3 0 0 1 0 1 
4 0 0 0 1 0
```

