[[그래프(Graphs)]]의 각 노드에 인접한 노드들을 연결리스트(Linked List)로 표현하는 방법이다.

즉, 노드의 개수만큼 인접리스트가 존재하며, 각각의 인접리스트에는 인접한 노드 정보가 저장된다. 그래프는 각 인접리스트에 대한 헤드포인터를 배열로 갖는다.

- 헤드포인터: 연결 리스트의 맨 처음 노드를 가리키는 포인터


![[Pasted image 20230919232038.png]]

가중치가 없는 [[무방향 그래프(Undirected graph)]]
: 그림과 같이 가중치 표현 없이 인접한 노드 정보가 저장된다.

가중치가 있는 [[유방향 그래프(Digraph(Directed graph)]]([[가중치 그래프(weighted graph)]])
: 종점 `[노드 번호 | 간선의 가중치]` 정보가 저장된다.


```python
V, E = map(int, input().split()) # 정점의 갯수와 간선의 갯수를 입력받음  
  
edge = list(map(int, input().split())) # 간선의 정보를 입력받음  
  
adj = [[] * (V + 1) for _ in range(V + 1)]
# 첫번쨰 인덱스 0을 무시하기 위해 정점 + 1 x 정점 + 1 갯수만큼 이차원 리스트를 초기화  
  
for i in range(E):  
    n1 = edge[i * 2] 
	# 첫번째 인덱스 0를 건너뛰고 받기 위해서 edge[0] edge[2] edge[4] edge[6] edge[8]
	
	n2 = edge[i * 2 + 1] 
	# 첫번째 인덱스 0를 건너뛰고 두번째 인덱스를 받기 위해서 edge[1] edge[3] edge[5] edge[7]
	
	adj[n1].append(n2) 
	# adk[edge[0]].append(edge[1]) 예시) 1 2 1 3 2 4 3 5 4 6 일때 두번쨰 리스트(1)에 2값을 넣음.  
    
    adj[n2].append(n1)  
  
print(adj)
```
